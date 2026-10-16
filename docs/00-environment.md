# Go-Change — Environment & Org Reference

_Project: **Go-Change** · Scenario: Global Enterprise Ecosystem Synchronization (GEES) — simulated M&A integrating HQ (Org A) + Subsidiary (Org B)._
_Last updated: 2026-10-17_

## Orgs and their roles

| Role | Alias | Identity | Edition | Notes |
|------|-------|----------|---------|-------|
| **DevHub** (mints scratch orgs) | `GoChangeDevHub` | fresh dedicated Developer Edition | Developer | **Never expires.** Dedicated to Go-Change. Dev Hub enabled. Default DevHub (🌳). |
| **Production / Org A (HQ)** | `GoChangeProd` | gopal.poddar@go.change · Org 00DgK00000dOKTmUAO | **Enterprise** | Instance CAN96. Can spawn sandboxes. ⚠️ **Trial expires 2026-11-14** (extendable, not indefinitely). |
| **Scratch** (build here) | `GoChangeScratch_One` | auto-generated | Developer (scratch) | Disposable, 7-day. Currently the default org. |
| **UAT Sandbox** (Phase 5) | `GoChangeUAT` | from GoChangeProd | — | Dev sandbox for rehearsal. Not yet created. |
| **Org B (Subsidiary)** (Phase 2+) | `GoChangeOrgB` | TBD | — | Separate production family. Integration target. Not yet created. |

## ⚠️ Key deadline
**GoChangeProd (Enterprise trial) expires 2026-11-14.** On expiry: create a fresh Enterprise trial, re-point the production alias. DevHub + repo + code are UNAFFECTED — that is exactly why the DevHub is a separate non-expiring DE.

## Naming convention (locked)
All orgs/project use **Go-Change**, NOT "GEES". GEES is only the scenario description. Aliases: `GoChangeProd` (prod, no hyphen — leave as-is), `GoChangeDevHub`, `GoChangeScratch_One`, future `GoChangeUAT` / `GoChangeOrgB`.

## Other orgs in `sf org list` (NOT part of Go-Change — ignore)
DevHubPlayground, MyPersonalSource (Marlabs work org), CustomerOrg, MaybeDevHub (broken), AgentDx, sfdxOrg (broken), DogstopiaDev2 / DogtopiaStaging (unrelated Dogtopia sandboxes).

## Security notes
- Public repo → zero secrets ever committed. `.gitignore` hardened (*.key/*.crt/*.csr/*.pem/creds/auth URLs/.sf/.sfdx).
- A plaintext-credentials file for gopal.poddar@go.change exists OUTSIDE the repo (parent folder) — recommend rotating that password.
- Certs for Phase 2 JWT/mTLS live in a parent folder, OUTSIDE the repo. Keep them out.
