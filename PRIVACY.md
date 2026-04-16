# VATSIM Copilot - Privacy Policy

*Last updated: April 15, 2026*

## Summary

VATSIM Copilot transcribes ATC audio locally using Whisper. Audio is not uploaded by VATSIM Copilot itself.

If you enable a cloud AI backend, matched ATC transcript text is sent to that provider for parsing and optional tip generation. If you use Local AI, transcripts are sent only to the local OpenAI-compatible server you configured.

The app also performs an anonymous startup telemetry/update check unless you opt out.

## Startup Telemetry

On startup, VATSIM Copilot may send one anonymous request containing:

| Field | Value | Purpose |
|---|---|---|
| Random ID | A generated UUID stored locally | Counts installs without tying them to your identity |
| App version | e.g. `0.2.1` | Used for update/usage tracking |
| OS name | `win32`, `darwin`, or `linux` | Helps understand platform usage |

The app does not intentionally send your callsign, flight plan, audio, transcript text, or API keys as part of this telemetry payload.

## How Startup Telemetry Works

The app sends the startup request to the Scarf Gateway. That request is used for anonymous usage counting and may also return update metadata when available. The app does not follow redirects from this request.

If the request fails, telemetry and update checking fail silently and do not block app startup.

## Random Install ID

A UUID is stored at:

```text
~/.vatsim_copilot_id
```

On Windows this resolves under your user profile, for example:

```text
%USERPROFILE%\.vatsim_copilot_id
```

Deleting this file resets the install ID.

## Opting Out

Set either of these environment variables before launching the app:

```text
SCARF_NO_ANALYTICS=1
DO_NOT_TRACK=1
```

If either variable is set to `1`, `true`, or `yes`, the startup telemetry request is skipped.

## Other Network Activity

Depending on which features you use, VATSIM Copilot may also connect to:

- Your selected cloud AI provider for parsing or tip generation
- Your configured Local AI server
- SimBrief to load flight plan/OFP data
- VATSIM-related data sources for controller and flight-plan enrichment
- Weather/ATIS data sources for departure, arrival, and alternate airport weather

These requests are feature-dependent and are separate from startup telemetry.

## API Key Storage

API keys are stored in the OS credential vault when available:

- Windows Credential Manager
- macOS Keychain
- Linux secret-service backends such as libsecret

If keyring support is unavailable, VATSIM Copilot falls back to storing obfuscated key values in the local settings file:

```text
%APPDATA%\VATSIMCopilot\settings.json
```

The fallback encoding is reversible obfuscation, not encryption.

## Third-Party Services

| Service | Role |
|---|---|
| Scarf Gateway | Anonymous startup telemetry / update check |
| AI providers (Claude, ChatGPT, Gemini) | Optional transcript parsing and tip generation |
| Local AI server | Optional local transcript parsing (user-configured) |
| SimBrief | Optional flight plan and OFP import |
| Weather / ATIS sources | Optional airport weather display |
| VATSIM/network data sources | Optional controller and route enrichment |

## Contact

Questions or concerns: open an issue at [github.com/SavBuilds/VatsimCopilot-Dist](https://github.com/SavBuilds/VatsimCopilot-Dist)
