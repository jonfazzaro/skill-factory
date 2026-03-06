---
name: refactoring-team-loop
description: Iterative code refactoring through progressive lenses via a worker-reviewer agent team, with a quality-check loop for a second pass.
disable-model-invocation: true
argument-hint: "[target-path]"
hooks:
  TeammateIdle:
    - hooks:
        - type: command
          command: "${CLAUDE_SKILL_DIR}/../refactoring-team/references/guard-idle-worker.sh"
---

STARTER_CHARACTER = 💎

This skill wraps `refactoring-team` with a quality-check loop. All lenses, prompts, and reviewer guides live in the `refactoring-team` skill — this skill only adds the evaluator and loop logic.

REFACTORING_TEAM_DIR = `${CLAUDE_SKILL_DIR}/../refactoring-team`

## Prerequisites

Agent teams must be enabled in settings:
```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```
If not set, offer to add it before proceeding.

## Setup

If $ARGUMENTS provided, use as target path. Otherwise ask for:
- Target path (files or folder to refactor)
- Test command to verify changes

Verify the target path exists and tests pass before proceeding.

## Launch Round 1

Generate a short random ID: `head -c 3 /dev/urandom | xxd -p | head -c 3`

Use it to name the teammates:
- Worker: `worker-ID` (e.g. `worker-a3f`)
- Reviewer: `reviewer-ID` (e.g. `reviewer-a3f`)

Read the spawn prompts from refactoring-team:
- Worker: `REFACTORING_TEAM_DIR/references/worker-prompt.md`
- Reviewer: `REFACTORING_TEAM_DIR/references/reviewer-prompt.md`

Before spawning, replace these placeholders in both prompts:
- `TARGET_PATH` -> actual target path
- `TEST_COMMAND` -> actual test command
- `LENSES_DIR` -> `REFACTORING_TEAM_DIR/references/lenses`
- `GUIDES_DIR` -> `REFACTORING_TEAM_DIR/references/reviewer-guides`
- `WORKER_NAME` -> the worker's name (e.g. `worker-a3f`)
- `REVIEWER_NAME` -> the reviewer's name (e.g. `reviewer-a3f`)

Append this to the reviewer prompt before spawning:

> **Loop override**: At wrap-up, say "Round complete" (not "Refactoring complete"). After writing REFACTORING-LOG.md, go idle. The manager will decide whether another round is needed.

Spawn both teammates.

Tell the user:
- Shift+Down cycles between teammates
- For split panes: set `teammateMode: "tmux"` in settings

## Wait for Round 1 Completion

When both teammates go idle, check that `REFACTORING-LOG.md` exists. This signals the round is complete.

## Quality Check

Spawn a quality checker to assess the result:

- Generate a new ID: `head -c 3 /dev/urandom | xxd -p | head -c 3`
- Name it `checker-ID` (e.g. `checker-c9d`)
- Read [references/quality-checker-prompt.md](references/quality-checker-prompt.md)
- Replace placeholders:
  - `TARGET_PATH` -> actual target path
  - `LENSES_DIR` -> `REFACTORING_TEAM_DIR/references/lenses`
  - `CHECKER_NAME` -> the checker's name
- Spawn the quality checker

When the checker goes idle, read `.refactoring-remaining-issues.md`.

## Loop Decision

If the "Worth Fixing" section in `.refactoring-remaining-issues.md` has meaningful issues, launch Round 2. If empty or trivial, refactoring is complete — tell the user and run `speak "Refactoring complete"`.

## Launch Round 2

- Generate new IDs for worker and reviewer
- Read the spawn prompts again from `REFACTORING_TEAM_DIR/references/`
- Replace all placeholders as in Round 1 (including the loop override for the reviewer)
- Read `.refactoring-remaining-issues.md` and prepend its "Worth Fixing" items to the worker prompt under a `## Priority Issues` header — these are what the worker should focus on first
- Spawn both teammates
- When both go idle and a new `REFACTORING-LOG.md` is written, refactoring is complete

Run `speak "Refactoring complete"` and tell the user.

Cap at 2 total rounds. Do not loop further.
