# Lens: Error Handling

Errors silently swallowed, error paths untested, or error types that do not communicate what went wrong.

## The Question

When something goes wrong in this code, does the error path help or hide?

## How to Spot

- Exceptions caught and silently swallowed: `catch (e) {}`
- Catch-all handlers that mask bugs by treating all errors the same
- Exceptions used for normal control flow rather than exceptional conditions
- Error messages that say "something went wrong" with no context
- Functions that return null or undefined to signal failure silently
- Error handling logic duplicated across many call sites

## Process

For each error handler, ask: if this error fires in production, will someone be able to diagnose the problem? For each function that can fail, ask: does the caller know it can fail, and what to do about it?

## Trade-off

Not every function needs elaborate error handling. Internal code that controls its inputs can trust them. The focus is on boundaries — where external input, network calls, file I/O, or user actions can produce failures that need clear handling.

## Go Deeper

What other error paths are silent or misleading? Where is error handling duplicated? Where do callers not know a function can fail?
