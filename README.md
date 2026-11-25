<div align="center">

# 🛡️ AuraSafe 
# [Live Demo](https://sprightly-mooncake-63b89f.netlify.app/)
### Personal Safety, Rapid SOS & Emergency Companion Progressive Web App (PWA)

[![PWA Ready](https://img.shields.io/badge/PWA-Installable-10B981?style=for-the-badge&logo=pwa&logoColor=white)](#quick-start)
[![Architecture](https://img.shields.io/badge/Architecture-Zero--Build_Single_File-38BDF8?style=for-the-badge)](#architecture--tech-stack)
[![Security](https://img.shields.io/badge/PIN_Security-SHA--256_Hashed-F43F5E?style=for-the-badge)](#security--default-codes)
[![Offline First](https://img.shields.io/badge/Offline-Service_Worker_%2B_Radar-F59E0B?style=for-the-badge)](#offline-resilience)

**AuraSafe** is a mobile-first, offline-resilient personal safety Progressive Web App engineered for high-stress, low-connectivity situations. It combines tactile SOS triggers, real ambient audio evidence capture, live OpenStreetMap safe-haven routing, a "Walk With Me" dead-man's switch timer, and a functional Stealth Calculator disguise into a single deployable web application.

</div>

---

## ✨ Key Features

### 🚨 1. Tactile SOS & Active Emergency Command
* **Dual-Mode Activation:**
  * **2-Second Tactile Hold:** Press and hold the SOS button with an animated SVG progress ring for immediate, accidental-tap-resistant activation.
  * **Single Tap Countdown:** Launches a 5-second visual, haptic, and Web Audio API beep countdown before dispatching alerts.
* **Live Telemetry Streaming:** Continuously streams high-accuracy GPS coordinates (`navigator.geolocation`) and device battery percentage.
* **Ambient Audio Evidence Vault (`MediaRecorder`):** Automatically records live microphone audio during an SOS session with real-time Web Audio `AnalyserNode` frequency visualization and instant `.webm` evidence download.
* **Multi-Channel Guardian Dispatch:**
  * **1-Tap Native SMS (`sms:`)** and **WhatsApp (`wa.me`)** emergency message launchers pre-populated with your Google Maps coordinates, Medical ID, and battery level.
  * **Production Webhook Integration:** Asynchronous `POST` (`keepalive: true`) dispatch to any custom backend endpoint.
* **SHA-256 PIN-Protected Deactivation:** Prevents unauthorized cancellation by an assailant; enforces a 30-second brute-force lockout after 3 failed attempts.

### 🥷 2. Rapid Response & Stealth Utilities
* **Stealth Calculator Disguise Mode:** Instantly transforms the app into a fully functional standard arithmetic calculator.
  * Typing your **Stealth SOS Code** followed by `=` (default: `911=`) **silently activates SOS** in the background without alerting anyone watching the screen.
  * Typing your **Stealth Exit Code** followed by `=` (default: `1234=`) restores the AuraSafe interface.
* **110dB Acoustic Siren & Visual Strobe:** Synthesizes a high-decibel dual-tone sweeping siren via the Web Audio API paired with an optional full-screen high-contrast strobe overlay.
* **Realistic Fake Incoming Call:** Triggers a full-screen incoming phone call interface with synthesized ringtone, custom caller personas (*Mom, Dad, Cab Outside, Local Police*), and an active connected call screen to safely exit uncomfortable situations.
* **One-Tap Emergency Helplines:** Instant dialer and copy drawer for verified 24/7 rapid-response numbers (`112`, `1091`, `181`, `100`, `108`).

### 🗺️ 3. Live Safety Radar & "Walk With Me" Timer
* **Real-Time Safe-Haven Discovery:** Queries the **OpenStreetMap Overpass API** around your live GPS location to plot nearby **24/7 Police Stations, Emergency Hospitals, Metro/Transit Hubs, and Pharmacies** with Haversine distance calculations, walking ETAs, and external Google Maps turn-by-turn navigation links.
* **Offline SVG Tactical Radar:** Automatically switches to a built-in interactive SVG Tactical Radar if map tiles are unreachable in low-signal environments.
* **"Walk With Me" Monitored Check-In:** Set a destination and arrival timer (5, 15, 30 min, or custom). Features a 1-minute warning alert and **automatically triggers full Emergency SOS** if you fail to check in before the timer expires.

### 👥 4. Trusted Circle, Medical ID & Audit Logs
* **Trusted Circle Management:** Add, edit, and prioritize emergency guardians with custom alert channel tags (`SMS`, `Live GPS`, `Auto-Call`).
* **First-Responder Medical ID:** Stores blood group, allergies, medical conditions, and hospital preferences—accessible directly on the Active SOS screen.
* **Tamper-Evident Incident Logs & DPDP Compliance:** Timestamped activity trail exportable as a `.txt` report, paired with a first-run DPDP privacy disclosure and 1-click full device data wipe.

---

## 🚀 Quick Start

AuraSafe is built with a **zero-build, single-file architecture** (`index.html`), requiring no bundlers or npm dependencies to run.

> **Important Browser Requirement:** Browser security policies require **HTTPS** or **`localhost`** to enable `navigator.geolocation`, `MediaRecorder` (microphone), Web Crypto `SHA-256`, and Service Worker PWA installation. Avoid opening `index.html` directly via `file://`.

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/aurasafe.git
cd aurasafe
```

### 2. Serve Locally over HTTP(S)
Choose any static file server:

```bash
# Option A: Using Node.js (npx)
npx serve .

# Option B: Using Python 3
python3 -m http.server 8080
```
Then open `http://localhost:8080` (or `http://localhost:3000`) in Chrome, Safari, Edge, or Firefox.

### 3. Deploy to Production
Because the entire frontend lives in `index.html`, you can deploy instantly with automatic HTTPS on:
* **GitHub Pages:** Settings → Pages → Deploy from `main` branch (`/root`).
* **Vercel / Netlify / Cloudflare Pages:** Drag-and-drop the repository folder with zero build configuration.

---

## 🔐 Security & Default Codes

All sensitive preferences are stored locally in `localStorage`, and your SOS deactivation PIN is hashed using the **Web Crypto API (`SHA-256`)** with a static application salt.

| Action | Default Code | How to Use | Customizable? |
| :--- | :--- | :--- | :--- |
| **Deactivate Active SOS** | `1234` | Enter on the 4-digit PIN pad when tapping *"I Am Safe"* | Yes (`Logs & PIN` tab) |
| **Stealth Silent SOS** | `911=` | Type `911` and press `=` inside Stealth Calculator mode | Yes (`Logs & PIN` tab) |
| **Exit Stealth Disguise** | `1234=` | Type `1234` and press `=` inside Stealth Calculator mode | Yes (`Logs & PIN` tab) |

---

## 🔌 Backend Webhook Integration

In the **Logs & PIN** tab, you can configure an optional **Production SOS Webhook URL** (e.g., `https://api.yourdomain.com/v1/sos/dispatch`). When SOS is triggered or deactivated, AuraSafe sends an HTTP `POST` request with `keepalive: true`:

```json
{
  "event": "SOS_ACTIVATED",
  "timestamp": "2026-11-24T00:55:00.000Z",
  "reason": "2-Second Tactile SOS Hold",
  "batteryLevel": 86,
  "location": {
    "lat": 28.6315,
    "lng": 77.2167,
    "accuracy": 8,
    "zone": "Verified Live GPS Position",
    "realGps": true
  },
  "medicalId": {
    "fullName": "Ananya Verma",
    "bloodGroup": "O+",
    "allergies": "Penicillin, Dust Mites",
    "conditions": "Mild Asthma (Carries Albuterol Inhaler)"
  },
  "guardians": [
    {
      "id": "c1",
      "name": "Meera Verma",
      "relationship": "Mother",
      "phone": "+91 98110 44210",
      "primary": true
    }
  ]
}
```

---

## 🏗️ Architecture & Tech Stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Core UI & Styling** | Semantic HTML5, Tailwind CSS (CDN), Inter & JetBrains Mono | Mobile-first responsive shell (`max-w-[480px]`) with high-contrast dark mode |
| **Mapping & Geospatial** | Leaflet.js, CARTO Dark Tiles, OpenStreetMap Overpass API, Custom SVG Radar | Interactive safe-haven discovery, Haversine routing, and offline radar fallback |
| **Audio & Haptics** | Web Audio API (`OscillatorNode`, `AnalyserNode`), `MediaRecorder`, Vibration API | Synthesized 110dB siren, fake ringtone, live mic recording, and tactile feedback |
| **PWA & Offline** | Dynamic Blob Web App Manifest, Service Worker, `localStorage` | Home-screen installability, offline shell caching, and persistent state |
| **Cryptography** | Web Crypto API (`SubtleCrypto.digest('SHA-256')`) | Client-side PIN hashing without plaintext storage |

---

## 🛣️ Production & App Store Roadmap

Want to take AuraSafe from a web PWA to native iOS/Android app stores with server-side dead-man's switches, Twilio/MSG91 SMS cascades, and cloud S3 audio streaming? See [`AuraSafe_Production_Roadmap.md`](./AuraSafe_Production_Roadmap.md) for the complete 8-week engineering blueprint.

---

## ⚠️ Safety Disclaimer

AuraSafe is designed as a personal safety companion and rapid-alert aid. It **augments—and does not replace—direct contact with official emergency services** (`112`, `911`, `1091`, or your local emergency dispatch). Always prioritize reaching a verified safe location and contacting local authorities directly when possible.

## 📄 License

Distributed under the **MIT License**. Free to fork, customize, and deploy for personal safety and community protection initiatives.
