# Go-Change — Phase 1 Review & Roadmap Ahead

_Last updated: 2026-10-17_

This doc is the "where are we / where are we going" checkpoint, written before deciding how to provision scratch-org features (Step 1.4).

---

## PART A — What's DONE in Phase 1 (Environment & Pipeline Init)

| Step | What | Status | Evidence |
|------|------|--------|----------|
| 1.1 | Dedicated DevHub created & authed (`GoChangeDevHub`), never-expiring DE | ✅ | 🌳 default DevHub in `sf org list` |
| 1.1b | Production confirmed = `GoChangeProd`, Enterprise, sandbox-capable | ✅ | OrganizationType = Enterprise Edition |
| — | Scratch org minted & opened live (`GoChangeScratch_One`) | ✅ | Opened in browser; default org |
| 1.2a | Git initialized, secrets check passed, public repo pushed | ✅ | github.com/Gopal55SysAdmin/project-go-change |
| 1.3 | Restructured into 3 modular unlocked packages | ✅ | `sf project deploy --dry-run` → "No changes to deploy" (no config errors) |

**Repo:** `project-go-change` (public, account Gopal55SysAdmin, commits authored gopaljidpoddar@gmail.com).
**Commits so far:** baseline → modular restructure. Clean history.

### The modular package topology (Step 1.3 result)
```
gochange-core          (default)  — shared objects, base fields, common Apex
gochange-sync-engine   depends → core — Platform Events, Org A<->B sync Apex
gochange-experience    depends → core — Experience Cloud site, Omni-Channel, messaging
```
Why modular: deploy one module without the others; explicit dependencies; mirrors team ownership. Trade-off: more upfront structure (justified for an enterprise-scale portfolio).

---

## PART B — What's LEFT in Phase 1

| Step | What | Needs |
|------|------|-------|
| 1.4 | Harden `config/project-scratch-def.json` with required features/settings | ← **NEXT (deciding approach)** |
| 1.5 | GitHub Actions CI pipeline: PR → spin scratch org → deploy → run Apex tests → report | depends on 1.3 + 1.4 |

---

## PART C — Roadmap ahead (which features each phase needs)

This is the key input for the Step 1.4 decision — it shows which scratch-org features map to which future phase.

| Phase | Build | Scratch-org features likely needed |
|-------|-------|-----------------------------------|
| **2 — Security & Connectivity** | Certs, mTLS trust chain, External Client Apps (Org A↔B) | Mostly org-config, not scratch features. Possibly `ExternalClientApps`-related. (🔵 design+doc first) |
| **3 — Sync Engine (backend)** | Apex, **Platform Events**, **Change Data Capture**, data transformation; Batch Apex + Bulk API 2.0 | `PlatformEventsCloud` / platform-event + CDC provisioning; standard Apex (default) |
| **4 — Front-end & Service** | **Experience Cloud** site, **Omni-Channel**, Messaging | `Communities` (Experience Cloud), `ServiceCloud`, `OmniChannel`, digital-experience settings |
| **5 — Go-Live sim** | Merge branches, deploy to UAT sandbox, data load, deploy to prod | No scratch features (uses sandbox + prod) |

### The Step 1.4 decision (to make next)
Three options for how to provision scratch features:
1. **All Phase 2–4 features now** — any future scratch org immediately capable; slower creation.
2. **Lean / per-phase** — minimal now, add each feature when its phase arrives; teaches feature→capability mapping.
3. **Core-now + documented roadmap** — enable broadly-useful basics now, leave commented "add in Phase X" markers. (Balanced.)

_Recommendation on hold pending gopal's review of this doc._

---

## PART D — Depth tiers (time-boxed strategy)
- 🟢 BUILD+DEPLOY: CI/repo + one vertical sync slice (Account on Org A → Platform Event → Apex → upsert on Org B).
- 🟡 BUILD+DOC: Experience Cloud, Omni-Channel, Batch/Bulk reconciliation.
- 🔵 DESIGN+DOC: mTLS security (deep notes + diagrams first; full build later).
- Goal priority: (1) learn, (2) preserve (this /docs folder), (3) interview-ready.
