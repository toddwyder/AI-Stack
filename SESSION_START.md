# Session Start — read this first, every session

This file exists because chat memory is a lossy summary that goes stale, especially across context-window resets and Claude Code sessions that never get reported back to chat. Don't trust memory for anything below — verify live.

## Bootstrap (do this before anything else)

1. Pull both repos fresh: `toddwyder/AI-Stack` and `toddwyder/Julia`. Don't assume prior-session state is current.
2. Read, in this order: `PROCESS_IMPROVEMENT_WORKING_DOCUMENT.md` (the process itself — numbered sections, §2 has the 10 governing principles), then `RECONCILED_PROJECT_PLAN.md` (the actual Milestone A-E table with close conditions).
3. Check live issue state via GitHub API/`gh` CLI for Milestones B, C, D on AI-Stack, and Julia's own open issues — don't rely on this file's issue numbers as still-accurate without checking.
4. Then read "Where we left off" below.

## Standing rules (apply regardless of which milestone is active)

- Never state GitHub issue/milestone/board status as current fact from memory alone.
- Nothing gets relayed to Claude Code without a link or context pointer attached — a bare instruction with no attachment is how a prior session proposed recreating already-retired issues.
- Deletions, production Firestore writes, and anything irreversible require live Firestore access (Claude Code with real credentials, not this chat) and a real Type-2-gated process — never done from a chat sandbox.
- Update the "Where we left off" section below at the end of every session, before ending it.

## Where we left off (last updated 2026-08-17)

**Milestone A** (reconcile legacy issues) — done.
**Milestone B** (current-state read-in + pushback) — done. B1 and B3 both closed (AI-Stack#38, #41).
**Milestone C** (how-phase design pass) — **4 of 11 closed**: C-A (#39), C-D (#40), C-F1 (#37), C-E (#45).
  - **Unblocked, not started:** C-B (access scoping), C-C (Pocock skill mapping), C-F2 (choose Julia backlog migration mechanism — needs Todd's direct involvement, not pure chat work), C-G1 (data-integrity monitor), C-G2 (test/logging improvements), C-OBS (reviewer observability), C-DEPLOY (deployment mechanism).
  - **Still blocked:** C-H (needs C-DEPLOY first).
**Milestone D** — D0 (create the actual board) not yet done. D2's 12 migration issues are **filed but not started**: AI-Stack#42-44, Julia#6-14. Dependency order is written into each issue's body — Wave 1 (no blockers): AI-Stack#42/43/44, Julia#7 (Graphify), Julia#11 (schema decoupling — real production code, use the Agent/Reviewer split from AGENTS.md for this one).

**Real open findings, not yet resolved:**
- AI-Stack#46 — Julia has no logging/observability tooling (found during C-E). Not yet scoped, just recorded.
- `ISSUE_40_GSD_CORE_DECISION_REGISTER.md` §7 — AGENTS.md's Harness Roles section is temporarily in the wrong place; it should move to a shared canonical instruction source once D2.3 (AI-Stack#44) builds one. Don't "fix" this by deleting it — it's correctly there until #44 exists.
- Branch-protection enforcement on Julia main, and the AI-Stack/Julia Projects v2 board's actual contents, are both unconfirmed (token scope limits) — not evidence either way.

**Immediate next candidates:** C-G1/C-G2/C-OBS/C-DEPLOY are all newly unblocked by C-E closing. C-B and C-C have no dependencies and could start anytime.
