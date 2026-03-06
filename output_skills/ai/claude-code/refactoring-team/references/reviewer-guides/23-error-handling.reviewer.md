# Lens: Error Handling (Reviewer Guide)

## When to Apply

After mutable state. State management is clean — now check whether error paths are well-designed.

## What to Look For in Diffs

- Silent catch blocks given meaningful handling or removed
- Catch-all handlers narrowed to specific error types
- Error messages improved with context
- Duplicated error handling consolidated
- Functions that returned null for failure refactored to explicit error signaling

## First Pass

Worker should find obvious problems: silently swallowed exceptions, catch-all handlers, error messages with no context.

## Second Pass

If error handling is still unclear:
- Look for functions that can fail but give no signal to callers
- Check for error handling duplicated across call sites — could it live in one place?
- Look for exceptions used as control flow for non-exceptional conditions

## Push Back On

- Adding elaborate error handling to internal code that controls its inputs — trust internal boundaries
- Wrapping every call in try/catch when the caller cannot meaningfully handle the error

## When Done

Move on when error paths help diagnose problems rather than hiding them.
