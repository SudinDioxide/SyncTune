# SyncTune — Multi-Device Synchronized Music Player

![SyncTune Banner](https://img.shields.io/badge/SyncTune-Hi--Res%20Web%20Audio-6366f1?style=for-the-badge&logo=soundcharts&logoColor=white)
![WebRTC Sync](https://img.shields.io/badge/Sync-WebRTC%20%7C%20BroadcastChannel-06b6d4?style=for-the-badge)
![Storage](https://img.shields.io/badge/Database-IndexedDB%20%2B%20LocalStorage-10b981?style=for-the-badge)
![Deploy](https://img.shields.io/badge/Deploy-GitHub%20Pages%20Ready-ec4899?style=for-the-badge)

**SyncTune** is a single-file, zero-backend, multi-device synchronized music player. It decodes and plays high-resolution audio files (FLAC, WAV, ALAC, MP3, AAC, OGG) directly in your browser, synchronizes playback in real-time across laptops, phones, and smart devices via **WebRTC (PeerJS)** and **BroadcastChannel**, processes sound using a **3-Band Web Audio DSP Equalizer**, renders interactive **Canvas Spectrum Visualizers**, and stores your audio library offline using **IndexedDB as a local database**.

---

## ✨ Key Features

### 1. 🎵 Multi-Format Hi-Res Audio Engine
- **Supported Formats**: Local processing of `FLAC` (Free Lossless Audio Codec), `WAV` (Uncompressed PCM), `ALAC` (Apple Lossless), `MP3` (MPEG-3), `AAC` / `M4A`, and `OGG` (Vorbis).
- **Zero Cloud Uploads**: Files are decoded client-side with 32-bit floating-point Web Audio processing for maximum dynamic range and audiophile clarity.
- **Built-in Ambient Demo**: Comes pre-loaded with an offline procedural Web Audio synthesizer demo track so you can test audio, visualizers, and multi-device sync without needing immediate files.

### 2. ⚡ Real-Time Cross-Device Synchronization
- **PeerJS (WebRTC Data & Audio Streams)**:
  - Generate a 6-character room code (e.g. `TUNE-9842`) or instant QR code for mobile devices.
  - **Live Host Audio Streaming**: The host can broadcast the real-time audio stream directly to guest devices (`MediaStreamAudioDestinationNode`), allowing guests to listen in tandem even without having the original file!
  - **NTP-Style Clock Offset Compensation**: High-precision timestamp synchronization ensures microsecond-accurate playback across devices.
- **BroadcastChannel API Fallback**:
  - Instant zero-latency synchronization across browser tabs and multi-window setups on the same computer without requiring network or servers.

### 3. 🎚️ Web Audio DSP & 3-Band Equalizer
- **3-Band Tactile Equalizer**:
  - **Bass**: Low-shelf filter at 100 Hz (±15 dB)
  - **Mid**: Peaking filter at 1,000 Hz (Q = 1.0, ±15 dB)
  - **Treble**: High-shelf filter at 3,500 Hz (±15 dB)
- **Instant DSP Presets**: Flat, Bass Boost, Vocal Enhancer, Treble Air, Rock, Electronic.
- **Dynamics Limiter**: `DynamicsCompressorNode` safeguard to prevent digital clipping and audio distortion at high gains.

### 4. 📊 Audio Spectrum Visualizer
- Hardware-accelerated `<canvas>` visualizer rendered at 60 FPS:
  1. **Dynamic Bars**: Neon glowing frequency spectrum bars with peak hold indicators and mirror reflections.
  2. **Oscilloscope Wave**: Smooth time-domain bezier wave with glowing cyber aura.
  3. **Radial Spectrum**: Circular pulsing frequency spikes radiating around the spinning vinyl album art disc.
- **Multiple Color Themes**: Cyberpunk Neon, Sunset Synthwave, Emerald Aurora, and Royal Violet.

### 5. 🏷️ Metadata & Album Art Extraction
- Built-in `jsmediatags` parser extracts embedded ID3v2, ID3v1, and FLAC Vorbis comment tags:
  - Track Title, Artist, Album name, and Format details.
  - Embedded cover art rendered directly into the rotating vinyl disc.
  - Procedural dynamic gradient cover art generator when files lack embedded tags.

### 6. 💾 Local Storage as Database
- **IndexedDB (`SyncTuneDB`)**: Automatically stores audio files (Blobs) and metadata locally in your browser. When you refresh or return later, your uploaded songs and album covers persist offline!
- **LocalStorage**: Automatically saves your volume, visualizer preferences, loop mode, shuffle state, and customized EQ band values.

---

## 🏗️ Web Audio & Sync Architecture

```
[Local Audio File / Synth] ──► [HTMLAudioElement]
                                       │
                                       ▼
                         [MediaElementAudioSourceNode]
                                       │
                                       ▼
                       [BiquadFilterNode: Bass (100 Hz)]
                                       │
                                       ▼
                       [BiquadFilterNode: Mid (1000 Hz)]
                                       │
                                       ▼
                      [BiquadFilterNode: Treble (3500 Hz)]
                                       │
           ┌───────────────────────────┴───────────────────────────┐
           ▼                                                       ▼
[AnalyserNode (2048 FFT)]                                [GainNode (Volume)]
           │                                                       │
           ▼                                                       ▼
[Canvas Visualizer: Bars/Wave/Radial]                   [DynamicsCompressor Limiter]
                                                                   │
                                       ┌───────────────────────────┴───────────────────────────┐
                                       ▼                                                       ▼
                             [Speakers/Headphones]                                [MediaStreamDestination]
                           (AudioContext.destination)                                          │
                                                                                               ▼
                                                                                  [WebRTC PeerJS Stream Call]
                                                                                               │
                                                                                               ▼
                                                                                  [Guest Devices / Mobile Phones]
```

---

## 🚀 Deploy on GitHub Pages

Because **SyncTune** is a complete, self-contained single-file application (`index.html`), it can be deployed on **GitHub Pages** in less than 60 seconds.

### Method 1: Instant GitHub Pages Setup (Recommended)

1. Create a new repository on [GitHub](https://github.com/new) (e.g. `SyncTune`).
2. In your terminal or command prompt inside this folder, run:
   ```bash
   git remote add origin https://github.com/<YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY_NAME>.git
   git branch -M main
   git push -u origin main
   ```
3. On GitHub, navigate to your repository:
   - Go to **Settings** > **Pages** (under "Code and automation" in the left sidebar).
   - Under **Build and deployment** > **Source**, choose **Deploy from a branch**.
   - Select Branch: **`main`**, Folder: **`/(root)`**, and click **Save**.
4. In about 1 minute, GitHub will give you your live URL:
   `https://<YOUR_GITHUB_USERNAME>.github.io/<YOUR_REPOSITORY_NAME>/`

### Method 2: Automated GitHub Actions

This repository already includes `.github/workflows/deploy.yml`. When you push to the `main` branch, the GitHub Action automatically packages and deploys your site to GitHub Pages with zero configuration!

---

## 💻 Running Locally

You can test SyncTune locally using any standard HTTP server:

### Using Python:
```bash
python -m http.server 8080
```
Open [http://localhost:8080](http://localhost:8080) in your browser.

### Using Node.js:
```bash
npx serve -l 8080
```

---

## 📱 How to Use Multi-Device Sync

1. Open SyncTune on your primary device (Host).
2. Click **Sync Room** in the top navigation bar.
3. Your unique 6-character **Room Code** (e.g., `TUNE-A83B`) and a **QR Code** will be displayed.
4. On your mobile phone, secondary laptop, or friend's device:
   - Either scan the QR code with your camera app, or
   - Open SyncTune, click **Sync Room** > **Join Room**, and enter the Room Code.
5. Hit **Play** on the host device. Audio, timeline seeking, track changes, and visualizer events will sync across all connected screens!

---

## 🛡️ Privacy & Permissions

- **100% Client-Side**: Your songs never touch external servers or cloud storage.
- **Local Database**: IndexedDB stores your library strictly inside your local browser sandbox.
- **Peer-to-Peer**: WebRTC connects devices directly peer-to-peer via encrypted data channels.

---

## 📄 License

MIT License — Feel free to customize, host, and share!
