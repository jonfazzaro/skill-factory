# Lens: Coupling & Dependencies (Reviewer Guide)

## When to Apply

After semantic clarity. Now that the code reads well, look at the invisible wires between units — what depends on what, and why.

## What to Look For in Diffs

- Dependencies removed or narrowed (imports reduced, parameters simplified)
- Domain logic separated from framework/library internals
- Train wrecks shortened — intermediate objects no longer reached through
- Middle-man classes inlined or given real responsibility
- Interfaces introduced to break concrete dependencies

## First Pass

Worker should find the obvious: train wrecks, shotgun surgery patterns, framework code mixed into domain logic.

## Second Pass

If coupling still feels heavy:
- Trace a hypothetical change: "if I changed X, what else breaks?" If the answer is "many things," there's coupling the worker missed
- Look for circular dependencies between modules
- Check for classes that know about each other's internals
- Look for functions with long parameter lists that pass data through without using it — they're coupled to both ends

## When Done

Move on when dependencies feel intentional — each unit depends on what it genuinely needs and nothing more.
