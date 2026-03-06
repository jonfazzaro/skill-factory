# Lens: Coupling & Dependencies

Code that knows too much about too many things. A change in one place ripples unpredictably through many others.

## The Question

What does each unit depend on, and is that dependency necessary?

## How to Spot

- Train wrecks: `a.getB().getC().doThing()` — reaching through objects to get at something deep
- Shotgun surgery: one conceptual change requires edits across many files
- Inappropriate intimacy: classes that dig into each other's internals
- Framework or library details leaking into domain logic
- Middle man: a class that only delegates, adding indirection without value
- Circular dependencies between modules
- A function that imports a large module but only uses one small piece of it

## Process

For each file, ask:
1. What does this depend on? List the imports and the objects it reaches into
2. For each dependency — is it necessary, or could this unit do its job with less knowledge?
3. Where does one change force changes elsewhere?

## Trade-off

Some coupling is necessary — zero coupling means the code does nothing. The question is whether each dependency is *essential* (this unit genuinely needs it) or *accidental* (an artifact of how the code was written).

## Go Deeper

What other coupling exists? Where else does a change in one place force changes in another?
