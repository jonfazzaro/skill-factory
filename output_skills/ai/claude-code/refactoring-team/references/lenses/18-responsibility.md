# Lens: Responsibility & Feature Envy

Logic that lives in the wrong place — reaching into other objects or modules to do work that belongs there.

## The Question

Is each piece of logic in the place that owns the data it works with?

## How to Spot

- A function that uses several attributes of another class and none of its own
- Domain logic hiding in utilities, helpers, or service layers when it belongs on a domain object
- Logic duplicated across callers that could live once on the callee
- Classes named "Manager," "Helper," or "Handler" for what they manage rather than what they do
- The same problem solved in different ways in different places

## Process

For each function, ask: whose data does this work with? If it reaches into another object for most of its inputs, the logic probably belongs on that object instead.

## Trade-off

Not all logic belongs on the data it uses. Cross-cutting concerns, orchestration, and coordination are legitimately separate from the objects they work with. The smell is logic that is *envious* — constantly reaching for another object's internals.

## Go Deeper

What other logic is in the wrong place? Where are helpers doing work that belongs on a domain object?
