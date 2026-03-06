# Lens: Responsibility & Feature Envy (Reviewer Guide)

## When to Apply

After cohesion. Units are internally coherent — now check whether logic is in the right unit.

## What to Look For in Diffs

- Logic moved from services or helpers onto the domain objects that own the data
- Duplicated logic across callers consolidated onto the callee
- "Manager" or "Helper" classes renamed or eliminated

## First Pass

Worker should find obvious feature envy: functions that reach heavily into another object's data, domain logic hiding in utility layers.

## Second Pass

If misplaced logic remains:
- For each function, count whose data it uses most — if it uses another object's data more than its own, the logic likely belongs there
- Look for the same logic pattern appearing in multiple callers — it probably belongs on the object they all call
- Check for "orchestration" classes that contain business rules instead of just coordinating

## Push Back On

- Moving truly cross-cutting concerns (logging, orchestration, coordination) onto domain objects — those are legitimately separate

## When Done

Move on when logic lives with the data it works with, and services coordinate rather than compute.
