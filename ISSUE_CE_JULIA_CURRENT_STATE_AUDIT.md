# Issue C-E — Julia current-state audit

Status: complete. Synthesizes evidence already gathered in `CURRENT_STATE_READ_IN.md` (Milestone B1, 2026-08-16) and `ISSUE_40_GSD_CORE_RAW_FUNCTIONAL_CENSUS.md` (2026-08-17), plus one new check (logging/observability) not covered by either. Every row below is evidence, not assertion — sourced from live GitHub/repo state, not recollection.

## GitHub/project backlog

5 issues on `toddwyder/Julia` (#2-5 open, the dirt-list findings; #1 closed, a CI test artifact). `.planning/ROADMAP.md` is the real, current backlog — 71 phases logged (through the Phase 71 remediation phase added 2026-08-16). No Projects v2 board scoped to Julia specifically exists — the real board is a cross-repo view spanning both AI-Stack and Julia (confirmed by Todd, 2026-08-16), unverifiable in detail from this session (token scope).

## CI/GitHub Actions

Real and active, not aspirational. `pr-gate.yml` runs lint + type-check + unit tests + `test:regression` on every PR to main. `production-health.yml` runs an authenticated smoke test against live production after every deploy. `gsd-sync.yml` runs weekly, pulling upstream `gsd-core` via git subtree and auto-opening PRs (flagged in `ISSUE_40...` as needing explicit handling before gsd-core sunset — tracked as `toddwyder/Julia#9`, D2.7). Branch-protection *enforcement* (does a failing check actually block merge) is unconfirmed — API call needs a scope this session's token doesn't have. This is Phase 70 Success Criterion #1, still open.

## Tests, including integration coverage

Extensive. 40+ scripts in `package.json` spanning unit (`node --test`), integration (vitest), and e2e (Playwright). Dedicated `tests/`, `__tests__/`, `test/` directories, dedicated Firestore rules/schema test files.

## Logging/observability

**Real gap, confirmed this session.** No observability tooling of any kind — no Sentry, Datadog, LogRocket, PostHog, or equivalent in `package.json`. No centralized/structured logger utility found. Error/status reporting is 84 scattered `console.error`/`console.warn` calls across `app/api/` route handlers. On Vercel's serverless runtime, console output is not durable, searchable history by default — it's only useful if someone is actively watching at the moment something happens. There is currently no way to retroactively answer "did X error occur in production last week."

## Staging

Confirmed not built. No distinct staging config (separate Firebase project or Vercel staging environment) beyond a local emulator setup. This is Phase 70 Success Criterion #4. A local, non-production stand-in (`.firebaserc` pointing the Firebase CLI at an unrelated project, `wayfinder-10b9d`) was set up 2026-08-16 as a stopgap to keep Claude Code sessions from defaulting to production — explicitly not real staging, gitignored so it doesn't become mistaken for the real thing.

## Real integrations

Firestore confirmed real and live: `firestore.rules`, `firestore.indexes.json`, `firebase.json`, dedicated Firestore test files, production project `julia-44bd9` referenced directly in application code. As of the gsd-core census, `recipeDbSchema` and `clipperInboxSchema` are live-imported into Julia's own validation, Firebase write controller, and external-data-gateway code **from the vendored gsd-core package** — a real production runtime/compile dependency, not a process artifact. Tracked as `toddwyder/Julia#11`, D2.9.

## Deployment

Confirmed: `vercel.json` present; `production-health.yml` references the live URL `https://julia-mu-green.vercel.app`.

## Rollback/recovery

`HANDOFF_POST_ROLLBACK.md` exists at repo root — evidence at least one real rollback happened and was documented. No generalized/mechanized rollback *procedure* (script or runbook) found — each rollback appears to have been handled ad hoc.

## Production smoke testing

Confirmed real: `production-health.yml` runs an authenticated POST to `/api/health/production-verify` after every production deploy, failing the workflow on a non-200 response.

## Repo/model instructions

`CLAUDE.md` and `GEMINI.md` both exist at repo root, both substantial. `CLAUDE.md` was corrected 2026-08-17 (`toddwyder/Julia@3e3ef1e`) to point sessions at AI-Stack's process docs before proposing process/tracking work — previously told every session `.planning/STATE.md` was "the source of truth... read first," which was accurate for Julia's own phase work but caused a real incident (a Claude Code session proposed recreating already-retired AI-Stack issues) when applied to cross-repo process questions.

## Project memory

`.planning/learning.md` exists, was found duplicated (entire history repeated verbatim) with 2 entries corrupted during that duplication event, and was deduped/repaired 2026-08-16 (`toddwyder/Julia@3348d60`) — 87 lines to 47, no content lost. Root cause of the duplication/corruption itself was not identified — worth watching for recurrence once the write mechanism producing it is known.

## Access

Repo collaborators API shows only `toddwyder` with admin/push access — no other accounts or bots. As of 2026-08-16, Claude Code's Firebase CLI on the execution machine had live, unscoped write access to production Firestore by default (no barrier for any command not explicitly scoped to the emulator) — corrected same day (`.firebaserc` switched away from `julia-44bd9` as default). Production/staging access beyond GitHub and this Firebase CLI check was not independently verifiable from this session.

---

**Close when:** audit document exists covering all listed areas with evidence, not assertion. — Met. One real gap (logging/observability) has no existing tracking issue yet; two others (branch-protection enforcement, Projects board detail) are correctly marked unconfirmed rather than assumed either way.
