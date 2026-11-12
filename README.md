# badge/uploader

Send images, GIFs and videos from your computer or Android phone to a round Bluetooth **e-badge** – straight from the browser, without the manufacturer's app, without an account and without any cloud.

<p align="center">
  <img src="docs/desktop.png" alt="badge/uploader on a desktop browser" width="640">
  &nbsp;
  <img src="docs/mobile.png" alt="badge/uploader on an Android phone" width="200">
</p>

**Live version:** https://menrix.github.io/BadgeUploader/ – can be [installed as an app](#install-as-an-app-mac-windows-android).
(Or download `index.html` and open it in Chrome – the app itself is one single file.)

---

## Why badge/uploader?

Compared with the badge's original phone app (NN family / beamBox):

| | Original app | badge/uploader |
|---|---|---|
| Video length | cut to **3 seconds** | **as long as the badge's memory allows** (10 s and more tested) |
| Frame rate | fixed | choose it – smooth presets calculated from your video |
| Image quality | fixed | adjustable from 1 to 100, to make files smaller or sharper |
| Preview | of the picture only | **live preview of the real result**, incl. quality and frame rate |
| Not enough space | warning message | **checked before sending**, with one click to shorten to what fits |
| Crop | basic | move, resize **and rotate** freely |
| Runs on | iPhone / Android (iPhone app also on Apple-silicon Macs) | **any Mac, Windows PC, Chromebook or Android** with Chrome or Edge |
| Account | sign-up or guest mode | **none** |
| Internet | app with online features | **not needed** – works offline once loaded |
| Your files | handled by the manufacturer's app | **never leave your device** |
| Source code | closed | **open source** (MIT) |

## Features

- **Images, GIFs and videos** (JPG, PNG, WebP, GIF, MP4, MOV) – **videos longer than 3 seconds**.
- **Live preview of the real result** – the circle shows exactly what the badge will display, including the chosen JPEG quality and frame rate.
- **Crop** – move, resize and rotate. Desktop: drag, mouse wheel, double-click to reset. Phone: one finger to move, two fingers to resize and rotate, double-tap to reset.
- **Smooth frame rates** – the frame-rate buttons are calculated from the source video (every 2nd, 3rd, 4th frame …), so motion stays even. Your own value can be typed in (max. 20 fps, never more than the source).
- **Size check before sending** – the expected size is compared with the free space on the badge. If it does not fit, the send button turns red and offers to shorten the video automatically.
- **Clear error handling** – lost connection or failed upload are shown on the send button, with one click to reconnect or try again.
- **Works offline** – everything runs locally in your browser. Nothing is uploaded anywhere.

## Compatibility

| | |
|---|---|
| **Tested with** | NIKKO Dynamic E-Badge (1.8″ round IPS, 360 × 360 px) |
| **Should work with** | other Bluetooth badges that are used with the **NN family** or **beamBox** app (Beambox “NN Family” series, e.g. Nikko, Nano, Niji). These use the same round 360 × 360 display. |
| **Not tested** | Neo / Neo AI models (they also offer Wi-Fi transfer and may behave differently). |

If you try it with another badge, please open an issue and tell us whether it works.

## Requirements

Bluetooth in the browser (“Web Bluetooth”) is only available in Chromium-based browsers:

| Browser | macOS | Windows | ChromeOS | Android | Linux | iPhone / iPad |
|---|---|---|---|---|---|---|
| Chrome / Edge / Opera | ✅ | ✅ | ✅ | ✅ | ⚠️ may need to be enabled in `chrome://flags` | ❌ |
| Safari, Firefox | ❌ | ❌ | – | ❌ | ❌ | ❌ |

On iPhone and iPad, Safari, Chrome and all other regular browsers do not support Web Bluetooth (Apple requires all of them to use Safari's engine). Special browsers such as **Bluefy** (App Store) add Web Bluetooth themselves and might work – this is **untested**.
On macOS, Chrome may ask for Bluetooth permission the first time (System Settings → Privacy & Security → Bluetooth).

## Install as an app (Mac, Windows, Android)

badge/uploader can be installed like a normal app – with its own icon in the Dock / Start menu / home screen, its own window and offline start. Bluetooth works the same as in the browser.

- **Mac / Windows (Chrome or Edge):** open the live version and click the install icon at the right end of the address bar (or menu ⋮ → *Cast, save and share* → *Install page as app*).
- **Android (Chrome):** menu ⋮ → *Add to home screen* / *Install app*.
- **iPhone / iPad:** not possible – see [Requirements](#requirements).

Installing only works from the live version (https), not from a downloaded file.

## How to use

1. **Close the manufacturer's app** – the badge accepts only one connection at a time.
2. Open the page in Chrome and click **Connect device**. Choose your badge in the Bluetooth window.
3. **Drop an image, GIF or video** onto the page (or click to choose a file).
4. Adjust the crop, start and length, frames per second and quality. The preview shows the result.
5. Click **Send to badge**.

### Tips

- **File too large?** Lower the frames per second first (halves the size), then the quality.
- **Transfer stops?** Choose **Safe** as transfer speed.
- Uploading is limited by Bluetooth: roughly **30 KB per second**. A few seconds of video can take a minute.

## Privacy

The page has no server, no tracking and no account. Your files never leave your computer – they are converted in the browser and sent directly to the badge via Bluetooth.

## Limitations

- The badge's content cannot be listed, downloaded or deleted from the page (the Bluetooth commands for that are unknown). Use the badge's own menu to delete files.
- Gallery / slideshow mode of the original app is not supported yet.

## How it works

The Bluetooth protocol was worked out by recording the communication between the original app and the badge. It is plain and unencrypted: JSON commands plus JPEG images, split into small numbered packets with a simple checksum. Videos are sent as a sequence of JPEG frames.

Details: **[PROTOCOL.md](PROTOCOL.md)**

## Credits

Made by **Don_Key_Moustache**.
Built with the help of [Claude Code](https://claude.com/claude-code) (Anthropic).

## License

[MIT](LICENSE) – free to use, change and share, without any warranty.

---

*This is an independent project. It is not affiliated with, endorsed by or connected to Beambox, Shenzhen Guangsheng Artificial Intelligence Technology Co., Ltd. or the sellers of these badges. All product names and trademarks belong to their respective owners and are used only to describe compatibility.*
