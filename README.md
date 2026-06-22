# myReadOut — Universal Phone-Powered Screen Reader

> Point your phone at any screen. Hear it read aloud. No install required on the target device.

## Try it

Open `prototype/index.html` in Chrome on your phone. Paste your [Anthropic API key](https://console.anthropic.com) and tap the camera button.

That's it. It will read whatever your camera sees.

---

## The idea

Existing screen readers (VoiceOver, TalkBack) are great — but they only work on *your own device*, and only if you've set them up in advance. They can't read a kiosk, a friend's laptop, a TV, or an ATM.

myReadOut flips the model. **Your phone is the reader. Any screen is the target.**

Two modes (camera mode is working, tab audio is spec'd):

### 📷 Camera mode — working
Point your phone camera at any screen or printed text. The app uses Claude's vision API to extract and read the content aloud. Works on any physical display — monitors, kiosks, menus, signs, ATMs.

### 🎧 Listen to tab mode — spec'd, not yet built
On any device with a browser, open a URL or scan a QR code. The host browser shares its tab audio via WebRTC — no extension, no install. Your phone receives the stream, transcribes it in real time, and reads or summarizes it aloud.

---

## Overview 

| Tool | Camera read | Tab audio | No host install | Cross-platform |
|---|---|---|---|---|
| VoiceOver / TalkBack | ✅ | ❌ | ❌ (own device only) | ❌ |
| Seeing AI | ✅ | ❌ | ❌ | ❌ |
| Google Lookout | ✅ | ❌ | ❌ | ❌ |
| Apple Accessibility Reader | ✅ | ❌ | ❌ | Apple only |
| Copilot Vision | ✅ | Partial | ❌ (Edge only) | ❌ |
| **myReadOut (this)** | ✅ | ✅ (planned) | ✅ | ✅ |

The camera reading space is well-served. The gap is the **tab audio relay** — being able to listen to any browser tab on any device, with nothing installed on the host.

---

## Getting started (step by step)

Never used an API before? No problem. This takes about 5 minutes.

### Step 1 — Get an Anthropic API key

1. Go to [console.anthropic.com](https://console.anthropic.com) and create an account
2. Once logged in, click **API Keys** in the left sidebar
3. Click **Create Key**, give it a name like `myreadout`
4. Copy the key — it starts with `sk-ant-` — and paste it somewhere safe. **You only see it once.**

> **What does it cost?** Anthropic charges per use, not a monthly fee. Each capture costs roughly a few pence. You add credit to your account and it draws from that. There is no free tier, but a small amount of credit goes a long way. The only cost to you is your own use.

> **Is it safe?** Your key is stored only in your own browser. It never touches any server except Anthropic's directly. Nobody else — including this app — can see it.

### Step 2 — Open the app

1. Download or clone this repo
2. Open `prototype/index.html` in **Chrome** on your phone or computer
3. Paste your API key into the setup screen and tap **Get started**
4. Allow camera access when your browser asks

No server needed. No install. No account with us. Just a file.

### Step 3 — Use it

1. Point your camera at any screen, sign, document, or printed text
2. Tap the **camera button** in the middle
3. Wait a second — the app reads the text and speaks it aloud automatically
4. The detected text also appears on screen so you can read or copy it

**Tips:**
- Hold the camera steady and make sure the text is well lit
- Works best on clear printed text — handwriting is hit and miss
- Adjust the voice and speed in the settings at the bottom
- Tap the speaker icon on the right to replay the last result

---

## FAQ

**Does this work on iPhone?**
Yes — open it in Chrome on iPhone. Safari may have camera limitations so Chrome is recommended.

**What if I get an error about the API key?**
Double-check you copied the full key including `sk-ant-` at the start. If it still fails, your credit may have run out — check your balance at [console.anthropic.com](https://console.anthropic.com).

**Can other people use my key?**
No — each person who uses myReadOut needs their own key. The app is designed this way on purpose so your costs stay yours.

**Is my camera footage stored anywhere?**
No. A single frame is captured, sent to Anthropic's API for text extraction, and that's it. No footage is stored, recorded, or sent anywhere else.

**Can I use this offline?**
The camera capture works offline but the text reading requires an internet connection to call the API.

---

## How it works (camera mode)

1. Phone camera captures a frame via `getUserMedia`
2. Frame encoded as base64 JPEG
3. Sent to Claude's vision API with a prompt to extract text
4. Response spoken aloud via the Web Speech API
5. Text also shown on screen and copyable

### Tab audio mode (technical spec)

1. User visits `relay.myreadout.app` on the host device (any browser, no install)
2. Host browser requests tab audio via `getDisplayMedia({ audio: true })` — a standard browser API
3. Audio stream sent to a lightweight WebRTC relay server
4. Phone app joins the relay session via QR code or short link
5. Phone receives audio stream → Whisper (or equivalent) transcribes in real time
6. Phone speaks transcription or summarizes on demand

### Stack suggestion for tab audio
- **Relay server**: Node.js + mediasoup or simple-peer
- **Transcription**: OpenAI Whisper API or browser Web Speech API
- **Hosting**: Any VPS with WebSocket support

---

## What makes this different

- **No install on the host device** — just open a URL
- **Platform agnostic** — works on Windows, Mac, Linux, ChromeOS, smart TVs, kiosks
- **Phone-powered** — all AI runs from the cloud, not the host
- **Two vectors** — camera for physical screens, audio relay for live digital content
- **Summarization mode** — don't just read everything, tell me what matters

---

## Potential extensions

- Context memory across captures ("what did that last screen say about the price?")
- Language auto-detection + real-time translation
- "Describe this" mode for non-text content (images, charts, UI layouts)
- Haptic navigation cues
- Browser extension (optional) for deeper tab integration
- Smart glasses integration as a future hardware target

---

## Who this helps

- Blind and low-vision users navigating shared or unfamiliar devices
- Users with dyslexia or reading difficulties
- Non-native speakers needing translation alongside reading
- Anyone in a situation where they can't read a screen (bright sunlight, distance, hands occupied)
- Elderly users unfamiliar with built-in accessibility tools

---

## Status

Camera mode: **working prototype**
Tab audio relay: **spec'd, not yet built**

This is an open project, built by native coding / skilled coding student leveraging Claude AI for, literally, goodness' sake.

If you build on this, open an issue and say hey.

If it helped you, [pay what you want](https://buymeacoffee.com/jiandamonique) ☕

---

## License

MIT — do whatever you want with this.
