# Lens: Object & Type Design

Types that are not pulling their weight — anemic data bags, inheritance used where composition would be simpler, objects that can exist in invalid states.

## The Question

Are the types in this code carrying behavior and enforcing their own rules, or are they passive containers that other code manipulates?

## How to Spot

- Anemic objects: classes with only getters and setters, all logic lives in services
- Tell-Don't-Ask violations: code that asks an object for its state, makes a decision, then tells it what to do — the object should make the decision itself
- Classes that can be constructed in invalid states — invariants not enforced in the constructor
- Inheritance used where composition would be simpler: "is-a" where "has-a" fits better
- Missing value objects: a price is a float, not a Money type; a coordinate is two numbers, not a Point

## Process

For each type, ask: does it carry behavior, or do callers do all the thinking? For each decision made *about* an object, ask: could the object make this decision itself?

## Trade-off

Not everything needs to be a rich domain object. Data transfer objects, configuration records, and simple containers are legitimate. The smell is types that *should* carry behavior but do not — the logic exists, it just lives in the wrong place.

## Go Deeper

What other types are passive when they should be active? Where are callers making decisions that belong on the objects they query?
