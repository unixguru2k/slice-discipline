---
name: slice-discipline
description: Work in slices — decompose implementation plans into shippable increments, each with a named owner and a done test that proves the outcome (Verify lines for subagent delegation). Use when planning implementation work, breaking a feature into steps, delegating sub-tasks, deciding whether a coding turn is complete, or keeping a plan document current and archiving it when the work ships.
tags: [workflow, planning, delegation, verification]
version: 1.4.1
author: MACHAAO
---

# Slice Discipline

AI compresses *doing*, not *done*. Producing code is cheap; verifying,
integrating, and deciding whether it was worth building is not. The slice
is the working unit that closes that gap: a shippable increment with one
named owner and a done test that proves the outcome in the real world —
all six INVEST qualities of a good user story, plus the two things INVEST
leaves out (the named owner and the real-world done test).

Three disciplines:

## 1. Plan in slices

When presenting an implementation plan for approval (plan mode / your
plan-presentation tool), decompose the work into slices, each in this shape:

### Slice N: <name in domain language — "Use a saved card at checkout", not "add billing column">
- Ships: <what the user can do after this that they couldn't before>
- Touches: <layers/files>
- Done test: <the command/check that proves it — test cmd, build, /health after deploy>
- Owner: self | delegate(task=…, verify=…)

- **self** = you implement the slice in this session.
- **delegate** = hand the slice to a subagent (your agent's delegation
  tool — spawn_subagent, Task, …); the verify command is the done test it
  runs before reporting done.
- A plan with no done tests is not a plan — if you cannot name how a slice
  will be proven, research more before presenting.

## 2. Delegate with done tests

A delegated sub-task is incomplete without a done test. The subagent's
bounded context makes the done test the only cheap verification interface
between orchestrator and worker: you re-run one command to confirm instead
of reading anything.

Every delegation prompt ends with a `Verify:` line — the exact command or
check that proves the deliverable:

    Verify: pytest tests/test_auth.py passes

The subagent runs it itself before reporting done (it has shell access
unless denied).

## 3. Verify before done

A turn that implements or changes something is not complete until its done
test has run and passed:

- Run the tests (your shell tool with the project's test command), a
  build, or — after a deploy — verify the live `/health`.
- State the done test up front, before starting the work.
- If no done test exists or it cannot run, say so explicitly instead of
  claiming completion ("verification wasn't possible because …").

**Scope:** implementation turns only. Pure conversation, research, and
read-only ask-mode turns are exempt — no forced test-running on Q&A.

**Pacing:** one slice per turn. When working from a plan, implement the
next slice only — run its done test, record progress (section 4), then
stop and report. The user gates the next slice; never roll several slices
into one turn to save time — each slice exists so it can be verified and
reviewed on its own.

## 4. Record progress after every slice

A slice that ships but leaves no trace is invisible to the next session.
After each slice's done test passes, update the plan document (or the
tracking issue) before moving on:

- Mark the slice done and note what actually shipped (it may differ from
  what was planned).
- Record the done test that proved it — the command and its outcome.
- Note anything deferred or discovered (new constraints, follow-ups,
  open questions) so the next slice starts from reality, not memory.

This is one small edit per slice, not a status report: the plan file is
the running record, and the next session (or the next owner) reads it
instead of re-deriving state from git log.

**Where the plan lives:** plan documents are saved in the project's
`plans/` folder as `plans/<slug>.md` (lowercase, hyphenated slug — your
plan tool saves there automatically when it saves plans). When you start
working from a plan that isn't saved there yet — the user pasted one, or
it predates the convention — save it to `plans/<slug>.md` first, then
record each slice's outcome in that file. A plan that exists only in the
conversation is lost to the next session.

**When the plan is done:** once the final slice's done test passes, move
the plan document from `plans/` to `plans/archive/`. Active plans live in
`plans/`; completed ones in `plans/archive/` — a fully-shipped plan left
in `plans/` reads as open work to the next session. A plan with deferred
slices stays put until those land (its status line says what remains).

## 5. Execute from briefs, not the full plan

The plan's design sections (problem statement, rationale, alternatives,
open questions) exist for the planning/review phase. During execution they
are noise: re-reading them every turn burns context and invites the LLM to
re-litigate settled decisions instead of doing the slice.

Each slice therefore carries a compact execution brief:

    #### Execute (Slice N)
    - <imperative steps, pointing at design sections instead of restating them>
    - Done test: <the exact command>
    - Decisions frozen: design sections and resolved open questions apply as written.

During execution, read only the current slice's brief (plus a referenced
design section when the brief points to one). Do not re-derive intent from
the full plan, do not re-open open questions, do not redesign mid-slice —
if the brief is wrong or incomplete, stop and report the discrepancy to
the user instead of improvising a new design.

---

Source: "Own the Slice" — Kent Langley
(https://www.kentlangley.com/blog/own-the-slice/). The slice = INVEST
(Bill Wake, 2003) + a named owner + a real-world done test.
