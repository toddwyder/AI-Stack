# Legacy Phase-Hierarchy Reconciliation Mapping

**Milestone A deliverable (A2).** Produced retroactively — the reconciliation itself (A1, A3
in spirit) happened interactively across a chat session rather than via a single dry-run
script pass; this document is the mapping artifact that should have preceded it. Written
against the 23 open legacy Phase-hierarchy issues identified in `toddwyder/AI-Stack` as of
2026-08-16 (Discussion #32).

Out of scope: the 4 "ChatGPT Discussion Outbox" issues (#29, #31, #33, #35) are not part of
the legacy Phase-hierarchy and were not reconciled here.

| # | Title | Disposition | New home |
|---|---|---|---|
| 1 | Phase 0.1 — GitHub setup | Closed — literal duplicate of #2 (confirmed by author comment) | — |
| 2 | 0.1 — GitHub setup | Closed — superseded by new board model | Milestone D0 (structured plan) |
| 3 | 0.2 — Data baseline | **Open** — real Julia audit work, not in conflict | Closure is a Milestone B (current-state) question |
| 4 | 0.3 — Gating issue — Phase 0 mechanical exit | **Open** — findings now linked (Julia#2–5); close call is Todd's | — |
| 5 | 0.4 — Gating issue — Phase 0 capability exit | Closed — duplicate of #16 (already closed, "list is correct") | — |
| 6 | 0.1a — Create the migration Project board | Closed — superseded | Milestone D0 |
| 7 | 0.1b — Load phases 1–5 | Closed — superseded (epic-issue pattern explicitly retired, §3) | — |
| 8 | 0.1c — Load Phase 0.2's task-issues onto the board | Closed — superseded | Milestone D0 / C-F2 (Julia backlog migration mechanism) |
| 9 | 0.1d — Reps checkpoint (capability rep) | Closed — superseded (no discrete capability-exit concept in new model) | — |
| 10 | 0.2a — Define the read surface | **Open** — real Julia audit work, not in conflict | Closure is a Milestone B question |
| 11 | 0.2b — Confirm the canonical reference | **Open** — real Julia audit work, not in conflict | Closure is a Milestone B question |
| 12 | 0.2c — Run the fresh read-only audit | **Open** — real Julia audit work, not in conflict | Closure is a Milestone B question |
| 13 | 0.2d — Sort deviations into contamination types | **Open** — real Julia audit work, not in conflict | Closure is a Milestone B question |
| 14 | 0.2e — Reconcile reality against old docs | Already closed (pre-session) | — |
| 15 | 0.2f — Produce the verified dirt list | Already closed (pre-session) | — |
| 16 | 0.2g — Product-judge read of the list | Already closed (pre-session, "list is correct") | — |
| 17 | 1.1 - Mechanical exit | Closed — superseded + monitor design question resolved | Bucket-3 deletion decision → Julia ROADMAP.md Phase 69 |
| 18 | 1.2 - Capability exit | Closed — superseded | — |
| 19 | 2.1 - Mechanical exit | Closed — split | Generic template/pattern/doctrine → Milestone C-H; Julia instantiation → Julia ROADMAP.md Phase 70 |
| 20 | 2.2 - Capability exit | Closed — superseded | — |
| 21 | 3.1 - Mechanical exit | Closed — split | Codex/Pocock/cross-review → Milestone C-A/C-C; blast-radius tiering, pool-discipline routing, spend caps, loop-observability trail → AI-Stack backlog §8 |
| 22 | 3.2 - Capability exit | Closed — superseded | — |
| 23 | 4.1 - Mechanical exit | Closed — superseded, direct contradiction resolved | gsd-core audit → Milestone C-D; `learning.md` retained per §9/§10, NOT replaced by retrospective (old issue's claim doesn't carry forward) |
| 24 | 4.2 - Capability exit | Closed — superseded | — |
| 25 | 5.1 - Mechanical exit | Closed — superseded/relocated | Grand Regression → §13.I, a post-Milestone-D triggered activity, tracked in Milestone E |
| 26 | 5.2 - Capability exit | Closed — superseded | — |

## Summary

- **17 closed** (1, 2, 5, 6, 7, 8, 9, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26), each with a
  disposition comment on the issue itself.
- **6 left open** (3, 4, 10, 11, 12, 13), each with a comment recording why: real Julia
  audit work, not superseded by the new document, and closeable only after Milestone B
  confirms the underlying work actually happened.
- **3 backlog items added** to `PROCESS_IMPROVEMENT_WORKING_DOCUMENT.md` §8: blast-radius
  tiering, spend caps, loop-observability trail (commits `6ae0cbf`, `925de4c`).
- **2 new ROADMAP.md phases added** to `toddwyder/Julia`: Phase 69 (Bucket-3 stray-doc
  deletion, commit `b43b7a0`), Phase 70 (Julia CI/staging instantiation, commit `5771cc5`).
- **1 direct contradiction surfaced and resolved**: old #23 claimed the phase-close
  retrospective replaces `learning.md`; the current document (§9, §10) retains `learning.md`
  separately from both the milestone-close retrospective (§7) and mini-retrospective (§6).
  Resolved in favor of the current document.

## Post-change verification (A4)

- [x] All 23 legacy issues accounted for above (17 closed + 6 open = 23).
- [x] Every closed issue has a disposition comment on GitHub, not just in this file.
- [x] Every open issue has a comment recording why it's open.
- [x] Every "content preserved, not lost" claim traces to a real commit (linked above), not
      just an assertion.
- [x] No legacy Phase-hierarchy issue left open by accident with no comment explaining why.
