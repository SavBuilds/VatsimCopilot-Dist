## What's New in v0.2.3

### Update Notifications
- App now checks for updates automatically on launch
- An amber **↑ vX.X.X available** badge appears in the footer when a newer version is released
- Click the badge to open the release page directly

---

## Changes Since v0.1.0-beta

### v0.2.2 — Weather Panel
- Departure, arrival, and alternate weather now fetched simultaneously — worst-case load time reduced from ~42 s to ~12 s
- Temperature shown in the stats row
- Flight category badge (VFR / MVFR / IFR / LIFR) added below the source badge
- TAF section no longer clipped when expanded
- Stale utterances waiting more than 4 seconds in the transcription queue are now dropped automatically (busy frequency handling)
- Dynamic beam size under backlog — Whisper temporarily switches to greedy decode when 2+ utterances are queued to catch up

### v0.2.1 — Stability & Window
- Fixed a fatal Windows access violation on launch on some configurations
- Startup splash screen eliminates the white-flash artifact on launch
- Window size and position saved and restored between sessions
- Minimum window size enforced (800 × 560) so no UI content is ever clipped
- Close confirmation dialog when exiting while actively listening
- Window chrome buttons updated to Segoe MDL2 Assets glyphs (matches Windows titlebar)

### v0.2.0 — Alternate Weather & Controller Info
- Alternate airport weather from SimBrief OFP displayed in the weather panel
- DEP / ARR / ALTN segmented toggle on the weather card
- Controller info popup on hover — shows full ATIS text and clickable links
- ATC controller list refreshes silently every 30 seconds
- Enroute CTR controllers filtered by proximity to departure/arrival airports (400 nm)

---

## Installation
1. Download `VATSIM_Copilot_Setup_v0.2.3.exe` below
2. Run the installer — Windows SmartScreen may appear, click **More info → Run anyway**
3. Whisper model downloads automatically on first run (~470 MB)

## Privacy
Anonymous usage ping sent on launch (random UUID, app version, OS name — no personal data).  
Opt out: set `DO_NOT_TRACK=1` in your environment.  
[Privacy Policy](https://savbuilds.github.io/VatsimCopilot-Dist/PRIVACY)
