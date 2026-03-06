# Lens: Object & Type Design (Reviewer Guide)

## When to Apply

After coupling. Dependencies are clean — now check whether types carry their own weight.

## What to Look For in Diffs

- Data classes gaining behavior (methods that enforce invariants or make decisions)
- Value objects introduced for domain concepts that were raw primitives
- Tell-Don't-Ask refactorings: callers asking objects to act instead of querying and deciding
- Inheritance replaced with composition where appropriate

## First Pass

Worker should find obvious anemic types: classes with only getters/setters, callers that query state and make decisions the object should make itself.

## Second Pass

If types are still passive:
- Look for decisions made *about* objects that the objects could make themselves
- Check constructors — can the type be created in an invalid state?
- Look for missing value objects: are there concepts represented as raw numbers, strings, or tuples that deserve a type?

## Push Back On

- Over-enriching simple data transfer objects or configuration records — not everything needs behavior
- Introducing inheritance where simple composition or standalone implementations would be clearer

## When Done

Move on when types enforce their own invariants and carry the behavior that belongs with their data.
