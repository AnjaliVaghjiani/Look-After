<div align="center">

# Look After

### Making museum visits more accessible, one artwork at a time.

**An interactive, voice-guided museum experience designed with visually impaired visitors in mind.**

Built as an academic project at **Politecnico di Torino** (2024–2025).

`React Native` · `Expo` · `JavaScript` · `Node.js` · `Express` · `Google Cloud Speech-to-Text`

</div>

---

## The idea

A museum visit should be about discovering art, not struggling to find your way through it.

**Look After** explores how a mobile application can support greater independence during a museum visit. It combines spoken instructions, an artwork-focused guided journey, camera interactions and voice input in a single mobile experience.

The prototype uses an **Italian-language flow**, featuring *David* and the *Mona Lisa* as example artworks. It is an academic prototype, not a certified navigation or mobility aid.

## What you can explore

| Feature | How it works in the prototype |
| --- | --- |
| 🔊 **Spoken guidance** | Italian text-to-speech introduces screens, reads instructions and can replay audio. |
| 🖼️ **Artwork discovery** | Browse and select featured artworks before starting the guided visit. |
| 🧭 **Step-by-step directions** | View and hear predefined, artwork-specific walking instructions. |
| 📸 **Camera interaction** | Capture and preview photos during the museum journey. |
| 💬 **Artwork conversations** | Explore an artwork-themed chat experience with predefined, keyword-based replies. |
| 🎙️ **Speech input** | Record audio and send it to a backend that calls Google Cloud Speech-to-Text. |
| ♿ **Accessibility-oriented interactions** | Audio controls, spoken prompts and explicit navigation actions support the project's accessibility goal. |

> **Scope note:** Directions are scripted in the current code; the repository does **not** implement real-time indoor positioning, obstacle detection, or automated image recognition. The chat uses predefined responses rather than a generative AI model.

## A journey through the app

1. **Welcome:** launch Look After and hear the spoken introduction.
2. **Capture your surroundings:** open the camera and preview an image.
3. **Choose an artwork:** browse available pieces and select the one to explore.
4. **Follow the guided path:** read or listen to short, step-by-step directions.
5. **Confirm your arrival:** move through the artwork arrival and photo-confirmation screens.
6. **Learn and interact:** explore artwork information or use the artwork chat, including optional speech input.

The sequence reflects the screen navigation implemented in the app; physical location and arrival are not automatically verified.

## Technology

| Layer | Tools | Purpose |
| --- | --- | --- |
| Mobile app | React Native, Expo SDK 52, React Navigation | Screens, navigation and cross-platform application structure |
| Accessibility / media | `expo-speech`, `expo-camera`, `expo-av`, AsyncStorage | Spoken guidance, photos, audio recording and local UI state |
| Backend | Node.js, Express | Lightweight speech-to-text API endpoint |
| Speech processing | Google Cloud Speech-to-Text REST API | Transcription of recorded speech |

### Architecture

```text
                   LOOK AFTER MOBILE APP
                   React Native + Expo
                            │
          ┌─────────────────┴─────────────────┐
          │                                   │
   Guided museum experience             Voice interaction
   • artwork selection                  • record audio
   • scripted directions                • encode audio
   • artwork information                • submit recording
   • camera and preview                       │
   • predefined artwork chat                  ▼
          │                           Node.js / Express
          │                           POST /speech-to-text
          │                                   │
          │                                   ▼
          │                          Google Speech-to-Text
          │                                   │
          └───────────────────────────────────┘
                        Transcribed text
```

**Important:** The speech-to-text feature requires a running backend, an API key and a reachable server address. Other UI flows do not all depend on that backend.

## Repository structure

```text
Look-After/
├── client/
│   ├── App.js                 # App entry flow and navigation
│   ├── components/            # Screens, artwork flow, audio and camera UI
│   ├── config/                # Fonts and visual theme
│   ├── assets/                # Artwork images and app visuals
│   ├── android/               # Native Android project
│   ├── ios/                   # Native iOS project
│   └── package.json
├── server/
│   ├── server.js              # Express HTTP server
│   ├── speechToText.js        # Google Speech-to-Text API call
│   └── package.json
└── README.md
```

## Running locally

### Prerequisites

- Node.js and npm compatible with this project's Expo/React Native versions.
- Android Studio / an Android emulator, or an appropriately configured physical development device. An iOS native build requires macOS and Xcode.
- A **development build** for native modules used by this app; don't assume the full app works in Expo Go.
- For speech transcription only: access to Google Cloud Speech-to-Text and an API key.

### 1. Start the mobile app

```bash
cd client
npm install
npm run android
```

For a preconfigured native development build, you may instead start Metro with:

```bash
npm start
```

`npm start` is configured as `expo start --dev-client`. These steps describe the repository configuration; the supplied archive has **not** been build-tested against a clean machine or current tooling.

### 2. Configure and start the speech-to-text server (optional)

```bash
cd server
npm install
```

Create `server/.env` locally (do not commit real keys):

```dotenv
PORT=4000
GOOGLE_SPEECH_TO_TEXT_API_KEY=YOUR_API_KEY
```

Then run:

```bash
node server.js
```

The server exposes:

- `GET /` — basic health response.
- `POST /speech-to-text` — accepts JSON with `audioUrl` (Base64 audio content) and `config` (Google Speech API recognition configuration).

**Before running speech input:** `client/components/functions.js` currently has a hard-coded local IP address in `rootOrigin`. Replace it with the reachable IP address of your development server, or refactor it to use environment-based configuration. A physical device and server must be network-reachable. The backend uses the Google Speech-to-Text API and may incur usage charges.

## Current limitations

Look After was developed as a **museum accessibility prototype**. The repository currently includes:

- Predefined museum routes, not live mapping or indoor localization.
- A small, hard-coded example artwork collection.
- Keyword-based artwork chat, not an AI chatbot.
- Speech-to-text dependent on an external API and local backend configuration.
- Native dependencies and older framework versions that may require updates to run on newer devices.

The project has not been independently verified here for production deployment or safe, autonomous navigation. Visitors should not rely on the prototype for obstacle avoidance or physical safety.

## What this project explores

Beyond the interface itself, Look After is an exercise in designing software around accessibility needs: coordinating multiple screens, audio feedback, camera flows, conversational interactions and a small cloud-backed service into a cohesive mobile experience.

## Next improvements

- Add annotated screenshots and a short demo video.
- Replace hard-coded network configuration with an environment-based setup.
- Improve accessibility testing with screen readers and users with visual impairments.
- Introduce automated tests and document supported devices.
- Explore reliable, validated indoor navigation in a future iteration.

---

<div align="center">

**Look After** — because access to art should be for everyone.

</div>
