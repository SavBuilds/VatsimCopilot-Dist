# VATSIM Copilot — Changelog

---

## v0.1.0-beta — 2026-03-26

### Highlights

#### GPU Acceleration — On-Demand Download
- Standard build now supports GPU inference without a separate installer, reducing distribution size from 1.8 GB to ~400 MB (compressed)
- Uncheck **Force CPU** in Settings → Audio to trigger an automatic CUDA runtime download (~528 MB)
- GPU mode reduces average processing latency from ~1,500 ms to ~770 ms (RTX 3080: 100% of cases under 1.5 s)
- Falls back to CPU silently if GPU is unavailable or download is skipped

#### Performance Mode Selector
- New **Fast / Balanced / High Accuracy** dropdown replaces the raw model name picker
- Fast = `base.en` (~100 MB VRAM), Balanced = `small.en` (~500 MB), High Accuracy = `medium.en` (~1.5 GB)
- Contextual hint shows VRAM requirement and download status inline

#### Streamer Mode
- New toggle in Settings → Advanced masks SimBrief username and VATSIM CID with dots
- Callsign is preserved for parsing; only display fields are masked

#### SimBrief OFP Viewer
- New button opens the full plain-text OFP in a frameless scrollable window
- No external browser required

#### Instruction History
- Last 5 parsed instructions shown as compact history cards below the current result
- Each card shows instruction type badge, summary, and confidence indicator
- Oldest entries are discarded automatically

#### Route-Aware Parser
- Parser now accepts `active_fixes` from SimBrief `all_waypoints` (SID/STAR + enroute)
- Crossing restrictions referencing off-route but valid published fixes now resolve as confident
- Benchmark: 50/50 confident parses with SimBrief loaded (was 48/50 without)

#### Partial Transcript Streaming
- Whisper segment text appears progressively as each segment is decoded
- Live feed updates word-by-word on multi-segment transmissions (departure clearances, etc.)

### Performance (v0.1.0-beta, rules-only)
| Stage | CPU | GPU (RTX 3080) |
|---|---|---|
| Avg processing latency | ~1,518 ms | ~770 ms |
| Cases under 1.5 s | 48% (24/50) | 100% (50/50) |
| Rules accuracy (FAA + ICAO) | 100% | 100% |

### Privacy & Telemetry
- Anonymous usage heartbeat added — sends random UUID, app version, and OS name on startup
- Opt out at any time with `DO_NOT_TRACK=1` or `SCARF_NO_ANALYTICS=1`
- Privacy policy: https://savbuilds.github.io/VatsimCopilot-Dist/PRIVACY
- First-run welcome message explains telemetry and links to the privacy policy

### VAD Improvements
- Silence gate reduced: 900 ms → 600 ms (saves ~300 ms per transmission)
- Tail gate reduced: 300 ms → 150 ms (saves ~150 ms per transmission)
- 2-of-4 sliding window vote prevents single noisy frames from resetting the silence counter

### Whisper Improvements
- `beam_size` reduced from 5 → 3 (~33% faster, equivalent WER on ATC phraseology)
- `num_workers` set to 2 for CTranslate2 pipeline parallelism (~10% speedup)
- Leading silence trim strips PTT keying delay before audio reaches Whisper (~150 ms saved)

### Build
- Single Standard build (~540 MB) replaces separate CPU and CUDA installers
- CUDA runtime delivered as an on-demand GitHub Release download
- Version bumped to `v0.1.0-beta`

---

## v0.0.6-alpha — 2026-03-19

### New Features

#### User Manual PDF
- Added `generate_manual.py` — generates a full illustrated PDF user manual (`VATSIM_Copilot_Manual.pdf`)
- Covers installation, first-time setup, audio configuration, SimBrief integration, parsing modes, live session workflow, tips & troubleshooting, and keyboard shortcuts
- `build_release.py` automatically copies the pre-built PDF into `dist/VATSIM_Copilot/` as part of the build pipeline
- Build validates PDF presence and reports size; warns if missing

#### Manual Flight Plan Entry
- Added **Edit Flight Plan** button to the sidebar — opens a dialog to manually create or correct all flight plan fields without requiring a SimBrief import
- Editable fields: Callsign, Origin ICAO, Dest ICAO, Aircraft, Cruise Alt, SID, Dep Runway, Route, STAR, Arr Runway, Squawk
- Changes applied immediately to the active flight plan and reflected on the flight card

#### Frameless Window
- Removed native Windows title bar for a clean, fully custom look consistent with the app's dark theme
- Custom chrome added to the header bar: minimise (`–`), maximise/restore (`□`), and close (`✕`) buttons; close turns dark red on hover
- Click and drag anywhere on the header bar to move the window
- Double-click the header to toggle maximise / restore

#### Frameless Dialogs
- Edit Flight Plan dialog is now frameless — matches the main window aesthetic
- Custom title bar with label and `✕` close button; fully draggable
- Border rendered on an inner child frame to avoid Qt's unreliable top-level border clipping on Windows

#### Taskbar Icon
- Application icon now correctly appears in the Windows taskbar and alt-tab switcher
- `SetCurrentProcessExplicitAppUserModelID` set before app creation so Windows treats the process as VATSIM Copilot, not python.exe
- Icon forced via `WM_SETICON` on the HWND after window creation for reliable display on frameless windows
- Both large (256×256) and small (16×16) icon sizes set explicitly

#### On-Demand AI Tips
- AI tips are no longer generated automatically after every parsed instruction
- A **💡 Get Tip** button now appears on the instruction panel after a parse — tips only fire when you explicitly click it
- Keeps AI calls local and intentional; no silent background requests on every transmission
- Button is only shown when an AI backend is configured and a parseable instruction is available

#### Audio Worker Renamed
- Internal audio subprocess renamed from `audio_worker.exe` to `_audio_worker.exe`
- The leading underscore makes it visually clear it is an internal component, not something users should launch directly

### Fixes
- **Cancel button hover** — Edit Flight Plan Cancel button now correctly highlights on hover; was inheriting the OK button's blue style due to Qt not supporting text-based attribute selectors on `QDialogButtonBox`
- **First-time setup PDF** — Removed "Enter your callsign" from the First-Time Setup section; callsign changes every flight and already appeared correctly in the per-session section

### Build
- Correct build order: `python generate_manual.py` → `python build_release.py`
- `build_release.py` runs the full pipeline (PyInstaller → patch_bundle → copy PDF → validate); no need to invoke steps separately

---

## v0.0.5-alpha — 2026-03-18

### Highlights

#### Audio Overhaul
- Much more reliable audio device detection across Voicemeeter, VB-Cable, and other virtual audio setups
- New **Cable Bridge** mode — hear ATC through your headset and transcribe it simultaneously
- Fewer crashes and dropouts in packaged builds
- Resolved ctranslate2 DLL conflicts that caused audio worker instability in v0.0.4

#### Faster Transcription
- Reduced end-to-end delay from ATC speech to parsed instruction card
- Better handling of mid-transmission pauses and broken transmissions
- Whisper artifact stripping improved (trailing "over", "out", "ATC" removed more reliably)

#### Smarter Callsign Detection
- Improved filtering of other aircraft traffic on busy frequencies
- More accurate fuzzy matching of spoken callsigns against filed callsign
- Expanded Whisper mishear variant library for common airline telephony

#### Manual Spoken Callsign
- Define exactly how your callsign sounds on the radio (e.g. "State Police 1", "Medevac Niner")
- Helps Copilot recognize non-standard or hard-to-transcribe callsigns that Whisper mishears consistently

#### Parsing Improvements
- Better word-to-number conversion for altitudes, headings, and speeds
- Cleaner ATC phrase normalization patterns
- Improved route and SimBrief flight plan awareness during parsing

#### Smaller Build Size
- v0.0.4 → v0.0.5: **453 MB → 151 MB** (zip) / **1.1 GB → 548 MB** (unpacked)
- Achieved by switching to optional AI backends and filtering unused binaries from the PyInstaller bundle
- Dual-exe architecture (`VATSIM_Copilot.exe` + `audio_worker.exe`) with shared `_internal/` DLL directory

### Known Issues
- Very busy frequencies can still cause partial misses on fast or overlapping transmissions
- Some non-standard phraseology may parse imperfectly

---

## v0.0.4-alpha — 2026-03-17

### Highlights

#### Major UI Overhaul
- New stacked layout system for smoother transitions between views
- Improved structure and organization of flight data across the sidebar and main panel
- Cleaner, more readable dropdowns, controls, and instruction cards
- Overall visual polish for a more modern, cockpit-friendly appearance

#### Smarter Instruction Parsing
- Parser significantly improved to handle real-world ATC phrasing variations
- Reduced incorrect assumptions during paused or broken transmissions

#### Improved Number Recognition
- Earlier and more reliable word-to-digit conversion
- Better handling of altitudes, headings, and speeds spoken in non-standard forms

#### Enhanced Taxi & Ground Logic
- Phonetic taxiway parsing (e.g. "Hotel, Echo" → H, E)
- Cleaner taxi route formatting and display
- Runway crossing detection (e.g. "cross runway 15")

#### VATSIM Altitude Sync
- Automatically detects and applies filed altitude changes from the VATSIM network
- Keeps cruise altitude in sync with ATC amendments without manual update

#### Route Awareness Improvements
- Tracks skipped waypoints and route notes for better accuracy during deviations and direct-to clearances

### Technical Changes
- Expanded internal data model (route notes, skipped waypoints)
- Improved retry and validation logic for numeric parsing
- General codebase cleanup and consistency improvements

### Known Issues
- Some complex taxi instructions may still have minor formatting quirks
- Heavy radio congestion can still introduce occasional transcription inconsistencies

---

## v0.0.3-alpha — 2026-03-14

### Highlights

#### Targeted Listening & Taskbar Alerts
- Copilot now filters frequency chatter to focus strictly on your callsign
- Windows taskbar flashes via `FlashWindowEx` when ATC calls you, even when the app is in the background

#### Enhanced Whisper Models
- Toggle in Settings between `base.en` and `small.en` Whisper models
- `small.en` offers significantly improved recognition of complex aviation phonetics on capable hardware

#### Audio Pre-Roll ("Clipping" Fix)
- Re-engineered audio engine with a high-performance ring buffer
- Captures audio milliseconds before speech is detected — the first word of an instruction ("Turn", "Cleared") is never clipped

#### Partial Transmission Guard
- Parser now waits intelligently when ATC pauses mid-sentence to check radar
- Prevents premature incomplete instruction cards from being displayed

#### SimBrief-Linked Environment Awareness
- Linking your SimBrief plan pre-loads every runway, taxiway, gate, and frequency for your airports
- Every SID, STAR, and waypoint along your route loaded as context for the parser
- AI cross-references what it hears against a "ground truth" map of your specific flight

#### Smarter Number Handling
- Custom logic for radio homophones: "Runway for left" → 4L, "Runway ate" → 8, "tree" → 3

#### Dynamic UI Scaling
- Fixed SimBrief route overflow bug — window now accommodates any route length

### Technical Changes
- Added `FlashWindowEx` support for reliable taskbar notifications
- Expanded internal fix-dictionary for common Whisper hallucinations in high-static environments
- Implemented 7-day cache for airport data to optimize memory usage in long sessions

### Known Issues
- `small.en` model may take 10–15 seconds to load on older CPUs
- High radio static can still occasionally trigger ghost transcripts

---

## v0.0.2-alpha — Initial Release

### Core Foundation

#### Offline AI Engine
- Integrated `faster-whisper` (`base.en`) to process ATC audio locally — no cloud streaming, no data upload, zero external latency

#### SimBrief Integration
- "Sync" feature pulls your active flight plan and primes the AI to recognize your specific waypoints and destination fixes

#### Intelligent Scratchpad
- First implementation of the Rules Engine — automatically identifies and organizes altitudes, headings, and squawk codes into a structured UI

#### Targeted Alerts
- Monitors the frequency for your callsign and flashes the taskbar when you are addressed

### Technical
- Standard WebRTC Voice Activity Detection for speech segmentation
- Audio engine runs in a dedicated subprocess for UI stability during heavy transcription

### Known Issues
- Fast controller speech could clip the beginning of words
- Long pauses mid-clearance could split one instruction into two cards
- Large SimBrief routes could overflow the UI card
