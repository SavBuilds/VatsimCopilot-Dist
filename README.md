# VATSIM Copilot

![Status](https://img.shields.io/badge/status-beta-blue)
![Platform](https://img.shields.io/badge/platform-windows-blue)
![Engine](https://img.shields.io/badge/speech-faster--whisper-green)
![Version](https://img.shields.io/badge/version-0.2.1-informational)

**VATSIM Copilot** is a real-time ATC assistant for VATSIM pilots. It listens to ATC audio, transcribes it locally using Whisper, filters for your callsign, and extracts structured instructions such as altitude, heading, speed, squawk, runway, and route information.

All audio transcription runs locally on your machine. Audio is never uploaded by VATSIM Copilot itself.

---

## How It Works

```text
ATC Audio -> Whisper (local) -> Callsign Filter / Stitching -> Rules Parser -> Instruction Card
                                                       |
                                                       -> Optional AI backend
                                                          (Claude, ChatGPT, Gemini, or Local AI)
```

Copilot keeps normal callsign filtering local before AI parsing. Optional Local AI support lets you use any OpenAI-compatible local server without sending transcripts to a cloud provider.

---

## Parsing Behavior

| Mode | Description |
|---|---|
| **Offline** | Fully local rules-based parsing. No API key required. |
| **AI Assisted** | Rules parser runs first, then an AI backend handles edge cases and unusual phraseology. |
| **AI Only** | Bypasses the rules engine, but still keeps local callsign filtering unless AI Raw debug is enabled. |
| **AI Raw Debug** | Debug-only mode that sends every heard transcript to the selected AI backend and bypasses the local callsign filter. |

---

## Audio Routing

Current audio routing options include:

- **Virtual Cable (Recommended)** for clean ATC-only capture
- **Virtual Cable + Bridge** when Copilot should also mirror ATC to your headset
- **System Loopback** to capture all system audio
- **Microphone Test** for direct parser testing
- **BEACN / Hardware Mixer** when compatible BEACN devices are detected

---

## Integrations

| Integration | What It Adds |
|---|---|
| **SimBrief** | Loads flight plan context, OFP text, alternate airport, route/runway context, and dispatch shortcuts |
| **VATSIM** | Live status, route revision enrichment, CID-aware flight plan matching, and controller/network context |
| **Weather / ATIS** | Departure, arrival, and alternate airport weather with ATIS/METAR fallback |
| **ATC Panel** | Nearby controllers with hover details, ATIS/info popups, and clickable links |
| **Navigraph DFD** | Better waypoint/fix recognition and route-aware parsing |

---

## Privacy

- Audio is transcribed locally and is not uploaded by VATSIM Copilot
- If you select a cloud AI backend, matched ATC transcripts are sent to that provider for parsing
- Telemetry/update checks can be disabled with `DO_NOT_TRACK=1` or `SCARF_NO_ANALYTICS=1`
- API keys are stored in the OS credential vault when available, with a local fallback only when keyring support is unavailable
- Full privacy policy: [savbuilds.github.io/VatsimCopilot-Dist/PRIVACY](https://savbuilds.github.io/VatsimCopilot-Dist/PRIVACY)

---

## Quickstart

1. Launch `VATSIM_Copilot.exe`
2. Open **Settings**
3. Choose an **Audio Routing Mode** and select the correct capture device
4. Enter your callsign in the **Live Session** panel
5. Optionally save your SimBrief username in **Settings -> Connections**
6. Click **Start Listening**

See `VATSIM_Copilot_Manual.pdf` for the full setup and workflow guide.

---

## License

Beta release — see repository for license details.
