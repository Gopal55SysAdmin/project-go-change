# Go-Change — Scratch Org Definition & Feature Roadmap

_Last updated: 2026-10-17_
_File: `config/project-scratch-def.json` (JSON — cannot hold inline comments, so the roadmap lives here.)_

## Current definition (basics only)
Edition: Developer · Language: en_US · Lightning Experience on.
Feature `EnableSetPasswordInApi` — lets us set a password on the scratch org via CLI if we ever need UI login.

**Why lean now:** adding Experience Cloud / Omni-Channel features we won't use for weeks only slows scratch-org creation and risks edition quirks. We add each feature **when its phase arrives** (teaches which feature → which capability).

## Features to ADD per phase (edit `features`/`settings` when you reach the phase)

### Phase 3 — Sync Engine (Platform Events, CDC)
Add to `features`:
- `"PlatformEventsCloud"` — if Platform Event capacity needs raising (standard platform events work by default; verify first).
Change Data Capture is enabled via a `settings` entry / metadata, not a simple feature flag — provision CDC for selected objects through metadata or Setup. Confirm at build time.

### Phase 4 — Front-end & Service (Experience Cloud, Omni-Channel)
Add to `features`:
- `"Communities"` — enables Experience Cloud (Digital Experiences).
- `"ServiceCloud"` — Service Cloud features.
- `"OmniChannel"` — Omni-Channel routing (often requires ServiceCloud).
Add to `settings`:
- `"experienceBundleSettings": { "enableExperienceBundleMetadata": true }`
- Omni-Channel settings as needed (`omniChannelSettings`).

### Phase 2 — Security & Connectivity
Mostly org *configuration* (certs, External Client Apps, named credentials) rather than scratch-org feature flags. Little/nothing to add here.

## How to validate after editing
```
sf org create scratch --definition-file config/project-scratch-def.json --alias GoChangeScratch_Test --duration-days 1
```
If it creates without error, the definition is valid. Delete the test org after: `sf org delete scratch --target-org GoChangeScratch_Test`.

## Gotchas
- JSON = no comments. Keep roadmap here, not in the file.
- Not every feature is valid on every edition; Developer-edition scratch orgs support most but not all.
- More features = slower org creation. Add only what the current phase needs.

## Validation log / gotchas encountered
- **2026-10-17:** Including `securitySettings.passwordPolicies.enableSetPasswordInApi` as a *setting* failed with `Property 'enableSetPasswordInApi' not valid in version 68.0`. Fix: removed it — the `EnableSetPasswordInApi` **feature** already covers this; don't duplicate it as a setting. Lesson: `features` and `settings` validate differently, and settings are API-version-sensitive. Always validate against a real throwaway scratch org.
