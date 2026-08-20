# Deterministic Contract and Status Specification

Source task: AI-Stack issue #52, first foundation item.

This is the minimum specification for the manual process. Keep it simple and revise only from observed friction or effectiveness.

## 1. What–How–Stop

Use the same structure for role prompts so behavior can be tuned over repeated runs without changing the basic framing.

### What

**What = Goal + required output.**

- **Goal:** satisfy the expected outcome of the authoritative issue.
- **Output:** produce the role's required work product plus its required output template/artifact.

The issue defines the issue-specific outcome. The role prompt does not invent or predefine technical scope or root cause.

### How

**How = the required process.**

For example, a developer process may require planning, tracing/diagnosis when needed, TDD, required checks, evidence production, and GitHub handoff. Other roles have their own prescribed process.

### Stop

**Stop = role-specific guardrails and escalation conditions.**

Stop rules define what the role must not do and the narrow conditions that require authorization or termination of the step. Persistent cross-task guardrails should live in shared instructions such as `AGENTS.md` / `CLAUDE.md` rather than being duplicated in every role prompt.

Examples of persistent guardrails include: do not fix unrelated defects; log them, and do not ask the PM to make routine technical decisions that belong to the engineering role.

## 2. Acceptance contract

The acceptance contract is:

> **The minimum deterministic evidence sufficient to prove that the issue's expected outcome has been met.**

Emphasis is on **minimum**. Do not collect evidence merely because it might be useful. Add evidence only when it is necessary to establish the goal or prevent a false pass.

The acceptance contract is defined before RCA and must not assume a root cause, implementation, service boundary, or repair.

## 3. RCA and observability

RCA follows the real request path using a correlation ID.

- Follow one correlation ID through every microservice involved in the request path.
- If a required hop is not observable, add the minimum tracing needed to make that hop observable before selecting a repair.
- Use the trace to identify the first incorrect or missing state/handoff.
- Do not require architecture modeling, a domain dictionary, or a seam registry.

Service boundaries discovered from the trace inform technical work such as integration testing, logging, and tracing. They do **not** change the acceptance requirements.

## 4. Seam classification and IDs

**L0/L1/L2 seam classes are not used.** They are not knowable before RCA and are not needed to define acceptance.

**Stable seam IDs are not used.** The process does not maintain a seam registry or architecture/domain dictionary. Service-to-service boundaries are identified directly from the correlation trace when relevant.

If repeated manual use later demonstrates a real ambiguity problem, revisit this decision from evidence rather than pre-modeling the system now.

## 5. Review timing

Review occurs after execution produces a reviewable candidate and its required evidence.

Blocked work is not sent for review. Preserve the current evidence and blocker, then resume review only when a stable, unblocked candidate exists.

No separate review-trigger taxonomy is required for the manual process.

## 6. Acceptance status

Use only three acceptance statuses:

- **Pass** — the minimum required evidence proves the goal was met.
- **Fail** — the evidence proves the goal was not met.
- **Blocked** — a valid pass/fail determination cannot currently be made because completion or testing is prevented.

Record the reason separately. Do not encode blocker subtypes or uncertainty categories into the status itself.

## 7. Operating principle

Keep this specification consistent across runs so the prompts and process can be tuned from observed behavior. Prefer the smallest rule that solves an observed problem; do not add process machinery in anticipation of possible future needs.
