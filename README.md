<p align="center">
  <img src="icons/icon-192.png" alt="" width="96">
</p>

<h1 align="center">badge/uploader</h1>

<p align="center">
  <b>Images, GIFs, videos and galleries for round Bluetooth e-badges – straight from the browser.</b><br>
  No manufacturer app · no account · no cloud
</p>

<p align="center">
  <a href="https://menrix.github.io/BadgeUploader/"><img src="https://img.shields.io/badge/▶_Open_the_app-FF8A3D?style=for-the-badge" alt="Open the app"></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-3BA7F5" alt="Version 1.0.0">
  <img src="https://img.shields.io/badge/license-MIT-lightgrey" alt="MIT license">
  <img src="https://img.shields.io/badge/runs_in-Chrome_·_Edge-555" alt="Runs in Chrome and Edge">
</p>

<p align="center">
  <img src="docs/desktop.png" alt="badge/uploader on a desktop browser" width="640">
  &nbsp;
  <img src="docs/mobile.png" alt="badge/uploader on an Android phone" width="200">
</p>

---

## ✨ Features

| Feature | |
|---|---|
| 🎬 **Long videos** | no 3-second limit – as long as the badge's memory allows |
| 🖼️ **Mixed galleries** | images, GIFs and videos in one slideshow, always looping |
| ⏱️ **Own timing** | time per image, single images can stay longer, videos can play 2×, 3× … |
| ✂️ **Crop** | move, resize and rotate – mouse, wheel or two fingers |
| 👁️ **Live preview** | shows the real result incl. quality, frame rate and slideshow |
| 💾 **Size check** | compared with the badge's free space before sending |
| 📂 **Your files** | straight from your computer, drag & drop, several at once |
| 🔒 **Private** | runs offline in your browser – nothing is uploaded anywhere |

## 🆚 Compared with the original app

| | NN family / beamBox app | badge/uploader |
|---|---|---|
| Videos | cut to 3 s | **any length** that fits |
| Galleries | images only, fixed 3 s | **images + videos**, own time per item, repeats |
| Files | phone photo library | **any file**, drag & drop |
| Quality & frame rate | fixed | **adjustable** |
| Runs on | iPhone / Android | **Mac, Windows, Chromebook, Android** |
| Account / cloud | sign-up or guest mode | **none** |
| Source code | closed | **open source** |

## 🚀 Get started

1. **Close the manufacturer's app** – the badge accepts only one connection.
2. Open the **[app](https://menrix.github.io/BadgeUploader/)** in Chrome or Edge and click **Connect device**.
3. **Drop files** onto the page – several files become a gallery.
4. Adjust, check the preview, click **Send to badge**.

> 💡 **Too large?** Fewer frames/sec first, then lower quality. **Transfer stops?** Choose *Safe* speed.
> Bluetooth is slow (≈ 30 KB/s) – a few seconds of video can take a minute.

<details>
<summary><b>🖥️ Which browsers work?</b></summary>
<br>

Bluetooth in the browser (“Web Bluetooth”) only exists in Chromium-based browsers.

| | macOS | Windows | ChromeOS | Android | Linux | iPhone / iPad |
|---|---|---|---|---|---|---|
| Chrome / Edge / Opera | ✅ | ✅ | ✅ | ✅ | ⚠️ enable in `chrome://flags` | ❌ |
| Safari / Firefox | ❌ | ❌ | – | ❌ | ❌ | ❌ |

On iPhone/iPad every browser uses Safari's engine, so it does not work. Special browsers like **Bluefy** might – untested.
On macOS, Chrome may ask once for Bluetooth permission (System Settings → Privacy & Security → Bluetooth).
</details>

<details>
<summary><b>📲 Install as an app</b></summary>
<br>

Own icon, own window, starts offline – Bluetooth works the same.

- **Mac / Windows:** open the app in Chrome or Edge → install icon at the right end of the address bar.
- **Android:** Chrome menu ⋮ → *Install app*.

Only works from the live version (https). Alternatively download `index.html` and open it in Chrome – the whole app is this one file.
</details>

<details>
<summary><b>🏷️ Which badges?</b></summary>
<br>

- **Tested:** NIKKO Dynamic E-Badge (1.8″ round IPS, 360 × 360 px)
- **Should work:** other badges used with the **NN family** or **beamBox** app (e.g. Nikko, Nano, Niji – same round 360 × 360 display)
- **Untested:** Neo / Neo AI (also have Wi-Fi, may behave differently)

Tried another badge? Please open an issue and tell us.
</details>

<details>
<summary><b>⚠️ Limitations</b></summary>
<br>

The badge's content cannot be listed, downloaded or deleted from here (those Bluetooth commands are unknown) – use the badge's own menu.
</details>

## 🔧 How it works

The protocol was worked out by recording the original app's Bluetooth traffic: plain JSON commands plus JPEG frames in small numbered packets. Videos and galleries are frame sequences; repeated pictures are stored only once. Details in **[PROTOCOL.md](PROTOCOL.md)**.

## 📜 Version history

- **1.0.0** – first release

## 🙌 Credits & license

Made by **Don_Key_Moustache** · built with the help of [Claude Code](https://claude.com/claude-code) (Anthropic)<br>
[MIT license](LICENSE) – free to use, change and share, without warranty.

<sub>Independent project – not affiliated with Beambox, Shenzhen Guangsheng Artificial Intelligence Technology Co., Ltd. or the sellers of these badges. Product names and trademarks belong to their owners and are only used to describe compatibility.</sub>
