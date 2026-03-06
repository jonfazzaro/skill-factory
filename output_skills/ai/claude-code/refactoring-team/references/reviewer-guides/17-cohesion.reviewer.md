# Lens: Cohesion (Reviewer Guide)

## When to Apply

After comments. Surface cleanup is done — now look at whether the contents of each unit belong together.

## What to Look For in Diffs

- Classes or modules split into more cohesive units
- Unrelated functions moved out of convenience groupings
- Methods grouped by the fields they share

## First Pass

Worker should find obvious cohesion problems: classes that are hard to name, modules exporting unrelated functions, functions with variables that split into non-interacting groups.

## Second Pass

If cohesion issues remain:
- Try the "one sentence without and" test yourself on each class or module
- Look at which methods use which fields — if methods cluster into groups that use different fields, the class may want to split
- Check for modules named "utils" or "helpers" that have grown into grab-bags

## When Done

Move on when each unit can be described in one sentence and everything inside it belongs together.
