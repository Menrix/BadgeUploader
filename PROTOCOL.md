# Bluetooth protocol of the round e-badges (NN family / beamBox)

Worked out by recording the traffic between the original app and a NIKKO Dynamic E-Badge
(Bluetooth LE HCI trace). Everything here was verified by rebuilding the recorded packets
byte for byte. Values marked *unknown* were seen but not understood.

## GATT

| | UUID (16-bit) | Properties |
|---|---|---|
| Service | `0x01F0` | |
| Write characteristic | `0x01F1` | write without response |
| Notify characteristic | `0x01F2` | notify (enable via CCCD `0x0100`) |

The connection negotiated an ATT MTU of 512, so packets of up to 505 bytes fit into one write.
There is no pairing, no encryption and no authentication.

## Packet framing (host → badge)

Every write is one packet:

| Bytes | Content |
|---|---|
| 1 | `0xF1` start marker |
| 1 | command: `0x06` image / announce, `0x05` video |
| 2 | total number of packets of this message, big-endian (`0` if the message fits into one packet) |
| 2 | packet index, big-endian, counting **down** to 0 (last packet = 0) |
| 2 | payload length, big-endian (max. 496) |
| n | payload |
| 1 | checksum: two's complement of the sum of all previous bytes (all bytes add up to 0 mod 256) |

A message's payload is split into chunks of 496 bytes.

There is no per-packet acknowledgement. The original app sends about one packet every 16 ms
(~30 KB/s). On macOS, write-without-response packets that are sent faster than the link can carry
are dropped silently and the badge then disconnects – pace the packets.

## Messages

Each upload consists of two messages:

1. **Announce** – command `0x06`, payload `{"type":6,"number":1}`
2. **Data**
   - single image: command `0x06`, payload `{"type":6,"data":` + *image container* + `}`
   - animation / video: command `0x05`, payload `{"type":5,"data":` + *video container* + `}`

The badge answers each complete message with the notification `{GetPacketSuccess}`.

### Image container ("IMB")

36-byte header, little-endian, followed by a JPEG:

| Offset | Size | Value |
|---|---|---|
| 0 | 4 | `"IMB\0"` |
| 4 | 4 | header size = 36 |
| 8 | 4 | total size (36 + JPEG length) |
| 12 | 4 | `11` (*unknown*, probably “JPEG”) |
| 16 | 2 | width = 368 |
| 18 | 2 | height = 368 |
| 20 | 4 | data offset = 36 |
| 24 | 4 | JPEG length |
| 28 | 8 | zero |

The JPEG itself is 360 × 360 px, baseline, 4:4:4 (no chroma subsampling), as produced by the
original app. The header says 368 × 368 although the image is 360 × 360 – this is copied as is.

### Video container

All numbers little-endian. For *n* frames:

| Offset | Size | Value |
|---|---|---|
| 0 | 4 | magic `0x12345678` |
| 4 | 4 | `16 × (n + 1) + 8` |
| 8 | 4 | number of frames *n* |
| 12 | 4 | display time per frame in ms (e.g. 50 = 20 fps) |
| 16 | 11 + 1 | name, always `"output/50ms"` + zero byte (the original app writes this even for other timings; the badge ignores it) |
| 28 | 4 | total size of the container |
| 32 + 16·i | 12 | frame name `"frame_00001."` (exactly 12 bytes, no terminator) |
| 44 + 16·i | 4 | offset of frame *i* |

Frames start at offset `16 + 16 × (n + 1)`. Each frame has a 32-byte header followed by the JPEG:

| Offset | Size | Value |
|---|---|---|
| 0 | 4 | offset of this frame |
| 4 | 4 | offset of the next frame (last frame → first frame) |
| 8 | 4 | `11` |
| 12 | 2 | width = 360 |
| 14 | 2 | height = 360 |
| 16 | 4 | data offset (frame offset + 32) |
| 20 | 4 | JPEG length |
| 24 | 8 | zero |

The original app converts videos with ffmpeg into JPEG frames at 20 fps and cuts them to 3 s.

### Gallery / slideshow

There is no separate gallery command. The original app sends a gallery as a normal **video container**: one frame per image, and the display time per frame set to the slideshow interval (e.g. `3000` ms for 3 s per image). Recorded with three images: header `78 56 34 12 | 48 00 00 00 | 03 00 00 00 | B8 0B 00 00`, 31 411 bytes in 64 packets – badge/uploader produces exactly the same bytes.

### Repeating frames without storing them twice

All frames of one upload share the same display time. To show one picture longer (or play a video twice), frames are repeated. Tested on the badge (NIKKO):

- **Every repeat needs its own 32-byte frame header** – the badge follows the *next* pointers of the frame headers, not the directory. Several directory entries pointing to the same header are played only once.
- **The header's data offset is honoured** – a repeated frame can point to the JPEG data of an earlier frame. The picture is stored once; each repeat costs only 48 bytes (16 directory + 32 header).
- Display times of 10 s per frame work.

badge/uploader uses this for galleries: images and videos are put on one time grid (the video frame time, or – for images only – the largest time that divides all image times), and repeats point back to the stored picture.
The badge itself accepted 10 s videos without problems; 30 fps played back with stutter.

## Notifications (badge → host)

Framing: `0xA0`, command byte, 3 more header bytes (*partly unknown*), JSON text, checksum.

| Command | Content |
|---|---|
| `0x0D` | status, about once per second: `{"type":13,"allspace":16384,"freespace":7980,"devname":"","size":"368,368",…}` – sizes in KB |
| `0x0B` | `{GetPacketSuccess}` – message received |

## Not known yet

- Listing, downloading or deleting files on the badge
- Gallery / slideshow uploads (the announce message has a `"number"` field, probably for this)
