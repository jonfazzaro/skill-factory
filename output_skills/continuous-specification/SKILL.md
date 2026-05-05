---
name: continuous-specification
description: Continuous Specification process for writing code by defining specifications before implementing. Use whenever adding any new code, unless the user explicitly asks to skip specs or the code is exploratory/spike. Also known as TDD, but framed around specification rather than testing.
---

# Continuous Specification

Specification is a design technique. Design emerges from usage, not speculation. Short feedback loops let you course-correct immediately. The resulting architecture is specifiable by design, not retrofitted. We are not trying to rush towards feature completion — it's important that the code is correct and well-designed. Be thorough and only add what specifications demand.

When starting, announce: "Using Continuous Specification skill in mode: [auto|human]"

MODE (user specifies, default: auto)  

- auto: DO NOT ask for confirmation or approval. Proceed through all steps without stopping.
- human: wait for confirmation at key points

STARTER_CHARACTER = ❗️ when writing a spec, ✅ when aligned (green), 🌀 when designing, always followed by a space

## Core Rules

1. ALL code changes follow Continuous Specification — feature requests mid-stream are NOT exceptions. Write the spec first, then code.
2. Write only one specification at a time — focus on the simplest, lowest-hanging fruit behavior.
3. Predict failures — state what we expect to fail before running specs.
4. Two-step red phase:
   - First: make it fail to compile (class/method doesn't exist)
   - Second: make it compile but fail the expectation (return wrong value)
5. Minimal code to align — just enough to make the spec green. If no spec requires it, don't write it.
6. No comments in production code — keep it clean unless specifically asked.
7. Run all specs every time — not just the one you're working on.
8. Design at the first opportunity when specs are green.
9. Specify behavior, not implementation — check responses or state, not method calls.
10. Push back when something seems wrong or unclear.

## Specification Types

- **Code Specifications**: specify isolated components and units. Written in Establish-Execute-Expect structure.
- **User Specifications**: specify critical paths across the whole system. Validate entire system behavior end to end.

Separate these concerns. Keep Code Specifications focused on a single component's public interface. Write User Specifications for critical user journeys.

## Specification Planning

1. Think about what the code you want to write should do.
2. Plan specifications as single-line `[SPEC]` comments. 

    Example:

    ```
    [SPEC] Zero plus a number is equal to that number
    [SPEC] Add two positive numbers
    [SPEC] Add two negative numbers
    [SPEC] Adds a negative and a positive number
    [SPEC] Division by zero is not allowed
    ...
    ```
    
3. Check completeness — walk through [ZOMBIES](references/zombies.md) explicitly:
   - Zero/empty cases covered?
   - One item cases covered?
   - Many items cases covered?
   - Boundary transitions covered?
   - Interface clarity verified?
   - Exceptions/errors covered?
4. If MODE is human, wait for confirmation after specification planning.

## Implementation Phase

1. Replace the next `[SPEC]` comment directly with a failing specification. No intermediate markers.
2. Structure specifications in Establish-Execute-Expect with empty lines separating each section (do not add section labels as comments).
3. Name specifications as: `expected_result_when_action_given_condition`.
4. Use a `describe`/`it` nested structure:
   - `Given_` blocks describe preconditions or data state.
   - `When_` blocks describe the event or user action.
   - Nest `Given_` blocks inside `When_` blocks to express the same action under different conditions.
   - Use `it_should_` to start spec method names.
   - Spec names should read as complete sentences when combined with their describe blocks.
5. Think through the expected value BEFORE writing the expectation. Trace the logic step by step.
6. Predict what will fail.
7. Run specs, see compilation error (if specifying something new).
8. Add minimal code to compile.
9. Predict expectation failure.
10. Run specs, see expectation failure.
11. Add minimal code to align.
12. Predict whether specs will pass and why. Run specs, see green.
13. Simplify. For each line/expression just added, ask: "Does a failing spec require this?"
    - If no spec requires it, delete it — or if it's necessary, add a `[SPEC]` comment for it.
    - Run specs after each simplification.
    - Repeat until every line is justified by a spec.
14. Design.
    - Reflect on the domain: is there a missing concept that would make the code more expressive? An object waiting to be extracted? A better way to model the problem?
    - Introduce domain concepts (new abstractions) only — add NO new behavior. Specs must still pass, and no new code should be added that doesn't have specs.
    - Think about improvements to expressiveness, clarity, simplicity.
    - Say `🌀 Starting design stage` and list planned changes.
    - Implement one at a time, run specs after each.
    - When done (or if none needed), say "🌀 Design complete".
15. Go to step 1 for the next `[SPEC]` comment. Repeat until all planned specs are passing.

## Specification Design Rules

- Never use mocking frameworks. Use [Nullable infrastructure wrappers](references/nullables.md) to isolate dependencies.
- One execution (act) per specification.
- One logical expectation per specification to clarify failures.
- Keep specification setup DRY but explicit.
- Create builders for complex object establishment.
- Write failing specifications before fixing bugs to prevent regression.
- Specify unhappy paths and edge cases, not just the happy path.
- Apply FIRST principles: Fast, Isolated, Repeatable, Self-verifying, Timely.

## Final Evaluation

1. Analyze the code written and think about specifications that might have been missed.
2. If there are any gaps, start the process for the missing specs from the beginning — starting from `[SPEC]` comments then following the process until done.
3. Is anything still hardcoded in the code that shouldn't be? Fix it, analyze gaps, and go back to earlier stages if needed.
4. Analyze code expressiveness and quality. If there's anything to improve, go to design phase.
