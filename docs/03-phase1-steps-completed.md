# Go-Change — Phase 1: Steps Completed (Command Log)

_Last updated: 2026-10-17 · Phase 1 = Environment & Pipeline Initialization_

A step-by-step record of everything done in Phase 1, with the actual commands, so it can be retraced or explained in an interview.

---

## 1.1 — Create & authenticate a dedicated DevHub
**Goal:** a never-expiring org with authority to mint scratch orgs.

- Signed up a fresh **Developer Edition** at developer.salesforce.com/signup (dedicated to Go-Change).
- In that org: Setup → Dev Hub → **Enable Dev Hub = On** (one-way toggle).
- Authenticated the CLI:
```
sf org login web --alias GoChangeDevHub --set-default-dev-hub --instance-url https://login.salesforce.com
```
- Verified: `sf org list --all` → 🌳 default DevHub on `GoChangeDevHub`, Connected.

**Concept:** DevHub = key-maker. It mints scratch orgs; it is NOT where you develop and NOT the source of truth.

---

## 1.1b — Confirm Production org (Org A / HQ)
- `GoChangeProd` (gopal.poddar@go.change) already connected.
- Verified edition:
```
sf data query --target-org GoChangeProd --query "SELECT Name, OrganizationType, TrialExpirationDate, InstanceName FROM Organization"
```
- Result: **Enterprise Edition**, trial expires **2026-11-14** (sandbox-capable).

**Concept:** Prod is a deploy TARGET, touched last. Kept separate from DevHub so trial expiry never kills the pipeline.

---

## Live demo — mint a scratch org
```
sf org create scratch --definition-file config/project-scratch-def.json --alias GoChangeScratch_One --set-default --duration-days 7
sf org open --target-org GoChangeScratch_One
```
- A brand-new org appeared in the browser in ~13s. Auto-authenticated (no login needed — minted through the CLI).

**Concept:** scratch org = disposable workshop. Default org flips to it (`--set-default`), so build-time commands target it.

---

## 1.2a — Git + GitHub (source of truth)
- Hardened `.gitignore` for a PUBLIC repo (added *.key, *.crt, *.csr, *.pem, creds, auth URLs on top of the default .sf/ .sfdx/ node_modules/).
- Initialized + first commit (at project root `Go-Change Org/`):
```
git init
git add .
git status                      # secret-safety check: no .key/.crt/.sf/.sfdx/creds staged
git commit -m "chore: initial Go-Change SFDX project baseline"
```
- Fixed commit identity (was auto-stamped with machine hostname):
```
git config --global user.name "Gopal Poddar"
git config --global user.email "gopaljidpoddar@gmail.com"
git commit --amend --reset-author --no-edit
```
- Connected GitHub account + created public repo + pushed:
```
brew install gh
gh auth login            # GitHub.com, HTTPS, browser  (account Gopal55SysAdmin)
gh repo create project-go-change --public --source=. --remote=origin --push
```
- Repo live: **github.com/Gopal55SysAdmin/project-go-change**

**Concept:** Git stores FILES, connects to GitHub (not to any org). GitHub = source of truth. The project folder is where Git (files) and the sf CLI (orgs) meet.

---

## 1.3 — Modular unlocked-package topology
- Replaced the single `force-app` with three packages:
```
gochange-core          (default)                — shared objects/fields/common Apex
gochange-sync-engine   depends → GoChange Core   — Platform Events, Org A<->B sync
gochange-experience    depends → GoChange Core   — Experience Cloud, Omni-Channel
```
- Rewrote `sfdx-project.json` (3 packageDirectories, dependencies, `sourceApiVersion` 67.0 → **68.0**).
- Validated:
```
sf project deploy start --dry-run --target-org GoChangeScratch_One   # → "No changes to deploy" (no config errors = pass)
```
- Committed:
```
git add -A
git commit -m "refactor: restructure into modular unlocked packages (core, sync-engine, experience)"
git push
```

**Concept:** modular = independent deploys + explicit dependencies (install core first, or sync-engine's references break). Trade-off: more upfront structure — justified for an enterprise-scale portfolio.

---

## 1.4 — Scratch org definition (validated)
- Wrote a lean, known-good `config/project-scratch-def.json` (Developer edition, en_US, Lightning on, `EnableSetPasswordInApi`).
- Per-phase features to add later documented in `docs/02-scratch-org-features.md` (JSON can't hold comments).
- Validation caught a real error: `securitySettings.passwordPolicies.enableSetPasswordInApi` → "not valid in version 68.0". Removed it (the feature already covers it).
- Re-validated with a throwaway org, then deleted it:
```
sf org create scratch --definition-file config/project-scratch-def.json --alias GoChangeScratch_Test --duration-days 1
sf org delete scratch --target-org GoChangeScratch_Test --no-prompt
```
- Committed definition + docs.

**Concept:** features vs settings validate differently; settings are API-version-sensitive. Always validate against a real throwaway scratch org.

---

## STILL PENDING in Phase 1
- **1.5 — GitHub Actions CI pipeline:** on every PR, spin a scratch org, deploy the branch, run Apex tests, report pass/fail. Needs a DevHub auth secret stored in GitHub (encrypted). Decision pending: build after the first Custom Object (so CI has real code to validate).

## Daily command cheat-sheet
```
sf config set target-org GoChangeScratch_One   # building
sf config set target-org GoChangeProd           # deploying
sf project retrieve start                        # org → files
sf project deploy start                          # files → org
git add -A && git commit -m "..." && git push    # files → GitHub
```
