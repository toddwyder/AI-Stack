# Milestone B1 — Current-State Read-In

**Mode 1** (per RECONCILED_PROJECT_PLAN.md): Claude reads AI-Stack/Julia's actual GitHub state
and drafts each row below; Todd corrects/fills what GitHub can't show (whether something that
looks configured actually works, disabled tests, tacit context). Neither side finalizes a row
alone.

For every finding, same four dispositions used in Milestone A — pick one per row, don't leave
it implicit:

- **Update working doc** — `PROCESS_IMPROVEMENT_WORKING_DOCUMENT.md` states something that's
  now factually wrong (like §10 vs. old #23 turned out to be — but this time it might go the
  other way)
- **Julia ROADMAP.md** — real Julia-specific work with no home (Phase 69/70 pattern)
- **AI-Stack backlog (§8)** — real AI-Stack-generic idea with no home (blast-radius pattern)
- **Confirmed accurate, no action** — checked, matches the working document, move on

Close condition (B1): read-in complete, corrections captured. Close condition (B2): working
doc updated or confirmed unchanged, with corrections logged — i.e. every row below needs an
actual disposition, not left blank.

---

## AI-Stack repo

| Area | What GitHub shows (Claude drafts) | Todd's correction | Disposition |
|---|---|---|---|
| Open issues beyond the reconciled 6 (#3,4,10-13) | Only the 4 ChatGPT Discussion Outbox issues (#29,31,33,35). No other open issues. | | |
| `ChatGPT Discussion Outbox` issues (#29, 31, 33, 35) — what are these, still relevant? | Three-party (§12) discussion transcripts: #29 metric-definition/reviewer-gate, #31 Julia backlog migration mechanism (surfaces that `bulk_issue_create.py` shouldn't be assumed final — see next row), #33 reconciliation handoff sequencing, #35 harness-interruption checkpoint-recovery rule. All open. Content of all 4 appears already absorbed into the working doc (§11, §14). Likely closeable now. | | |
| Any code/tooling actually committed (vs. only described in the working doc) | `tools/bulk_issue_create.py` does NOT exist — no `tools/` directory in the repo at all. It was described/discussed but never committed. | | |
| Projects board(s) — do "Refactor Workflow" / "Double Loop" still reflect reality, or are they pre-reconciliation artifacts themselves? | **Blocked, not absent.** Confirmed by Todd: the board is a cross-repo (user-level) Project spanning both AI-Stack and Julia, not a per-repo one — consistent with the GraphQL query being rejected outright (PAT lacks `read:project` scope) rather than returning empty. Whether "Refactor Workflow"/"Double Loop" still reflect reality is unverifiable from this session; needs a token with `read:project` scope or a direct UI check. | Confirmed exists (cross-repo), verification blocked on token scope. | Blocked — revisit with broader-scope token. |
| Milestones — "Phase 0," "Phase 1" etc. still exist as GitHub Milestones; do these need renaming/retiring now that the Phase-hierarchy is reconciled? | Confirmed still present, all state=open: Phase 0 (6 open/10 closed issues), Phase 1–5 (0 open/2 closed each). These are legacy artifacts of the retired hierarchy. | | |

## Julia repo — per §13.E's audit areas

| Area | What GitHub shows (Claude drafts) | Todd's correction | Disposition |
|---|---|---|---|
| GitHub/project backlog (currently: 4 issues, ROADMAP.md is real backlog) | Confirmed: 5 issues total — #1 closed ("Test: Broken Pipeline CI/CD Gate," looks like a CI test artifact not real work), #2–5 open (dirt-list findings). No Projects board confirmed either way (same scope limit as above). `.planning/ROADMAP.md` confirmed real and current — 70 phases logged, most recent are Phase 69/70. | | |
| CI/GitHub Actions | Real, not aspirational. 3 workflows: `gsd-sync.yml`, `pr-gate.yml` (lint + type-check + unit tests + `test:regression` gate every PR to main), `production-health.yml` (authenticated POST smoke test against live prod URL on every successful deploy). Whether branch protection actually *enforces* pr-gate.yml as a required check is unconfirmed — API returned 403 (PAT lacks scope), not evidence of absence. | | |
| Tests, incl. integration-test coverage | Extensive — 40+ scripts in package.json spanning unit (`node --test`), integration (vitest), and e2e (Playwright); dedicated `tests/`, `__tests__/`, `test/` dirs; dedicated Firestore rules/schema test files. | | |
| Logging/observability | Not confirmed either way from a file-listing pass — no dedicated logging config surfaced. Needs a deeper audit (C-E), not resolvable from repo structure alone. | | |
| Staging | Not confirmed as existing. No distinct staging config (separate Firebase project / Vercel staging env) found beyond local emulator setup. Consistent with Phase 70's own text calling Firestore-isolation-for-staging "an open item." | | |
| Real integrations (Firestore, etc.) | Confirmed real: `firestore.rules`, `firestore.indexes.json`, `firebase.json`, 3 dedicated Firestore test files, production project `julia-44bd9` referenced directly in code. | | |
| Deployment (Vercel per memory — confirm current) | Confirmed: `vercel.json` present; `production-health.yml` references live URL `https://julia-mu-green.vercel.app`. | | |
| Rollback/recovery | `HANDOFF_POST_ROLLBACK.md` exists at repo root — evidence at least one real rollback happened and was documented. No generalized/mechanized rollback *procedure* found (e.g., a script or documented runbook). | | |
| Production smoke testing | Confirmed real: `production-health.yml` runs an authenticated POST to `/api/health/production-verify` after every production deploy, fails the workflow on non-200. | | |
| Repo/model instructions (CLAUDE.md or equivalent) | Confirmed: `CLAUDE.md` (9.5KB) and `GEMINI.md` (9.7KB) both exist at repo root, both substantial. | | |
| Project memory (`learning.md` — does it exist yet, per §10?) | Exists at `.planning/learning.md`, 86 lines, populated with real phase-by-phase RCA notes through Phase 53. **Flag:** the tail contains a garbled/corrupted entry (text with spaces inserted between every letter) — looks like a write bug, not content you'd have written that way. Worth checking directly. | | |
| Access (who/what has write access to prod, staging, repo) | Repo collaborators API shows only `toddwyder` (admin/push). No other accounts or bots found. Prod/staging access not independently checkable from GitHub alone. | | |

## Open threads carried in from Milestone A (verify these actually happened)

| Item | Expected state | Confirmed? |
|---|---|---|
| Phase 69 (Bucket-3 stray-doc deletion) | Logged in ROADMAP.md, NOT yet executed — real Firestore delete still pending | **Confirmed matches expectation** — `[ ]` unchecked, "Depends on: Backlog." |
| Phase 70 (Julia CI/staging instantiation) | Logged in ROADMAP.md, NOT yet executed | **Confirmed logged as not-yet-done**, but `pr-gate.yml` CI already exists and runs today — worth reconciling whether Phase 70 means something more specific (branch-protection enforcement + staging specifically) than what's already live, so the phase isn't marked complete for the wrong reason or left open when part of it is actually done. |
| toddwyder/Julia#2-5 (dirt-list findings) | Still open, still accurate? Production drifts — re-check freshness per the original audit's own warning | **Not re-verified this session** — confirmed still open on GitHub, but freshness against live Firestore was not re-checked (would require re-running the audit scripts against production, out of scope for a GitHub-only read-in). |

---

## Summary — B1 closed 2026-08-16

- **Rows requiring working-doc updates:** none — the working document held up against live GitHub state on every row checked.
- **Rows requiring new Julia ROADMAP.md phases:** Phase 71 added (Bucket-1 mock-user-123 remediation, toddwyder/Julia#2) — the one real gap found, now filled.
- **Rows requiring new AI-Stack backlog items:** none.
- **Rows confirmed accurate, no action:** CI/Actions (with a progress note added to Phase 70), tests, Firestore integrations, deployment, production smoke testing, repo instructions (`CLAUDE.md`/`GEMINI.md`), `learning.md` existence, access (single-user), Phase 69/70 logging, Julia backlog (5 issues + ROADMAP.md).
- **Rows resolved with real action taken this session:**
  - `bulk_issue_create.py` didn't exist → resolved via AI-Stack#37 (C-F1, Ready-to-pull contract), which unblocks C-F2's mechanism decision.
  - `learning.md` was duplicated + 2 entries corrupted → deduped and repaired (commit `3348d60`), 87 → 47 lines, no content lost.
  - Outbox issues #29/31/33/35 → confirmed their content landed in the working doc (§5/15, §13.F, §14, §11 respectively) and closed with pointers.
  - Milestones Phase 1–5 → closed as retired (Phase 0 left open — it still holds 6 genuinely open issues: #3, #4, #10–13).
  - Phase 70 vs. existing CI → not a conflict; `pr-gate.yml` partially satisfies Success Criteria #2, noted directly in ROADMAP.md so Phase 70 execution extends it rather than rebuilding from scratch.
  - Julia#2 rewritten with a real close-when (delete + reconfirm, CI regression test proven red-then-green, integrity monitor alarm) — also became the worked example for the Ready-to-pull contract itself.
- **Rows still genuinely open, with a named home:**
  - Projects v2 board — confirmed to exist (cross-repo), contents unverifiable this session (token scope). Revisit with `read:project` scope or direct UI check.
  - Branch-protection enforcement on Julia main — unverifiable this session (token scope); this is Phase 70 Success Criterion #1, needs a live test regardless of token access.
  - Staging environment — confirmed not built. Phase 70 Success Criterion #4.
  - Logging/observability — not confirmed either way from repo structure. In scope for Milestone C's **C-E** (full Julia current-state audit), not yet run.
