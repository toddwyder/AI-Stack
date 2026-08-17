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
| Open issues beyond the reconciled 6 (#3,4,10-13) | | | |
| `ChatGPT Discussion Outbox` issues (#29, 31, 33, 35) — what are these, still relevant? | | | |
| Any code/tooling actually committed (vs. only described in the working doc) | | | |
| Projects board(s) — do "Refactor Workflow" / "Double Loop" still reflect reality, or are they pre-reconciliation artifacts themselves? | | | |
| Milestones — "Phase 0," "Phase 1" etc. still exist as GitHub Milestones; do these need renaming/retiring now that the Phase-hierarchy is reconciled? | | | |

## Julia repo — per §13.E's audit areas

| Area | What GitHub shows (Claude drafts) | Todd's correction | Disposition |
|---|---|---|---|
| GitHub/project backlog (currently: 4 issues, ROADMAP.md is real backlog) | | | |
| CI/GitHub Actions | | | |
| Tests, incl. integration-test coverage | | | |
| Logging/observability | | | |
| Staging | | | |
| Real integrations (Firestore, etc.) | | | |
| Deployment (Vercel per memory — confirm current) | | | |
| Rollback/recovery | | | |
| Production smoke testing | | | |
| Repo/model instructions (CLAUDE.md or equivalent) | | | |
| Project memory (`learning.md` — does it exist yet, per §10?) | | | |
| Access (who/what has write access to prod, staging, repo) | | | |

## Open threads carried in from Milestone A (verify these actually happened)

| Item | Expected state | Confirmed? |
|---|---|---|
| Phase 69 (Bucket-3 stray-doc deletion) | Logged in ROADMAP.md, NOT yet executed — real Firestore delete still pending | |
| Phase 70 (Julia CI/staging instantiation) | Logged in ROADMAP.md, NOT yet executed | |
| toddwyder/Julia#2-5 (dirt-list findings) | Still open, still accurate? Production drifts — re-check freshness per the original audit's own warning | |

---

## Summary (fill in once rows are complete)

- Rows requiring working-doc updates:
- Rows requiring new Julia ROADMAP.md phases:
- Rows requiring new AI-Stack backlog items:
- Rows confirmed accurate, no action:
