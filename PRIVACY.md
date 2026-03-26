# VATSIM Copilot — Privacy Policy

*Last updated: March 2026*

## Summary

VATSIM Copilot runs entirely on your local machine. Your audio, ATC transcriptions, flight plan data, and callsign are never uploaded anywhere. The only outbound network activity is an optional anonymous usage heartbeat and version check, described below.

---

## What We Collect

On startup, VATSIM Copilot sends a single anonymous request to check for updates and record that the app is in use. This request contains:

| Field | Value | Purpose |
|---|---|---|
| Random ID | A UUID generated on first run | Counts unique installs without identifying you |
| App version | e.g. `0.1.0-beta` | Determines whether an update is available |
| OS name | e.g. `Windows` | Understand which platforms are in use |

**We do not collect:** your name, email, IP address, callsign, flight data, audio, transcriptions, or any AI backend API keys.

---

## How It Works

The heartbeat is sent via [Scarf Gateway](https://scarf.sh), a privacy-focused analytics service used by many open-source projects. Scarf logs approximate geographic region (country-level only) and strips full IP addresses. The request then redirects to a GitHub-hosted version manifest that the app reads to check for updates.

---

## Your Random ID

A random UUID is generated the first time you run VATSIM Copilot and stored at:

```
~\.vatsim_copilot_id
```

This ID is not linked to your identity in any way. Deleting this file resets it. The ID persists across reinstalls so that a single install is not counted multiple times.

---

## Opting Out

Set either of the following environment variables before launching the app and no network request will be made:

```
SCARF_NO_ANALYTICS=1
DO_NOT_TRACK=1
```

---

## Third-Party Services

| Service | Role | Privacy Policy |
|---|---|---|
| Scarf Gateway | Anonymous usage analytics | [scarf.sh/privacy](https://scarf.sh/privacy) |
| GitHub | Version manifest hosting | [docs.github.com/site-policy](https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement) |

If you use an AI parsing backend (Claude, OpenAI, or Gemini), your ATC transcriptions are sent to that provider for processing. Refer to each provider's privacy policy. API keys are stored locally in `%APPDATA%\VATSIMCopilot\settings.json` and are never transmitted by VATSIM Copilot itself.

---

## Contact

Questions or concerns? Open an issue at [github.com/SvgBst0427/VatsimCopilot-Dist](https://github.com/SvgBst0427/VatsimCopilot-Dist).
