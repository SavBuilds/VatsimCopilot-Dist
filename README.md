# VATSIM Copilot

![Status](https://img.shields.io/badge/status-beta-blue)
![Platform](https://img.shields.io/badge/platform-windows-blue)
![Engine](https://img.shields.io/badge/speech-faster--whisper-green)
![Version](https://img.shields.io/badge/version-0.1.0--beta-informational)

**VATSIM Copilot** is a real-time ATC assistant for VATSIM pilots. It listens to ATC audio, transcribes it locally using Whisper, and extracts structured instructions — altitude, heading, squawk, routing, and more — so you never miss a clearance.

All audio transcription runs **entirely on your machine**. No audio is ever uploaded.

---

## What It Does

When ATC says:

> "DAL123 descend and maintain 5,000, turn left heading 240, reduce speed to 210."

Copilot shows:

| Field | Value |
|---|---|
| Altitude | 5,000 ft |
| Heading | 240° |
| Speed | 210 kts |

And generates a suggested readback.

---

## How It Works

```
ATC Audio → Whisper (local) → Callsign Filter → Rules Parser → Instruction Card
                                                      ↓
                                             Optional AI assist
                                          (Claude / GPT-4o / Gemini)
```

Callsign filtering happens locally before any AI call — unrelated traffic never reaches the AI backend.

---

## Parsing Modes

| Mode | Description |
|---|---|
| **Offline (Rules)** | Fully local, no API key needed. 100% accuracy on standard phraseology. |
| **AI Assisted** | Rules parser first; AI handles edge cases and unusual phrasing. |
| **AI Only** | All parsing delegated to the AI backend. |
| **Raw Debug** | Shows raw Whisper transcript with no filtering. |

---

## Performance

Validated on 50 FAA + ICAO cases (v0.1.0, rules-only):

| Metric | CPU | GPU (RTX 3080) |
|---|---|---|
| Avg processing latency | ~1,518 ms | ~770 ms |
| Cases under 1.5 s | 48% | 100% |
| Rules accuracy | 100% | 100% |

---

## GPU Acceleration

The Standard build supports on-demand GPU acceleration. When you disable **Force CPU** in Settings for the first time, the CUDA runtime (~528 MB) downloads automatically. No separate installer required.

---

## Audio Setup

### Recommended — VB-Audio Virtual Cable

```
vPilot audio output  →  CABLE Input
CABLE Output         →  Copilot (input device)
```

### Advanced — VoiceMeeter

```
vPilot → VoiceMeeter AUX Input → B1 bus → Copilot
```

Select **Voicemeeter Out B1** as the input device in Copilot. Do not select A1/A2/A3 (headset outputs).

---

## Integrations

| Integration | What It Adds |
|---|---|
| **SimBrief** | Route-aware parsing — crossing restrictions, SID/STAR fixes resolved automatically |
| **VATSIM** | Live flight plan sync, altitude and squawk cross-check |
| **Navigraph DFD** | Fix/waypoint recognition for improved parser accuracy |

---

## Privacy

- Audio is transcribed locally by Whisper — never uploaded
- If an AI backend is enabled, ATC transcriptions are sent to that provider
- An anonymous usage heartbeat is sent on startup (opt out: `DO_NOT_TRACK=1`)
- Full privacy policy: [savbuilds.github.io/VatsimCopilot-Dist/PRIVACY](https://savbuilds.github.io/VatsimCopilot-Dist/PRIVACY)

---

## Quickstart

1. Extract the distribution folder
2. Launch `VATSIM_Copilot.exe`
3. Set your audio input device in Settings → Audio
4. Enter your callsign in the Live Session panel
5. Click **Start Listening**

See `VATSIM_Copilot_Manual.pdf` for full setup and configuration instructions.

---

## License

Beta release — see repository for license details.
