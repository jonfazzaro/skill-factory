# Lens: Cohesion

A class, module, or function that contains things that do not belong together — it serves multiple masters.

## The Question

Does everything inside this unit belong together? Does it have one reason to exist?

## How to Spot

- A class whose methods split into groups that use different fields
- A module that exports unrelated functions grouped only by convenience
- A function whose variables are used in completely separate sections that do not interact
- A class that is hard to name because it does many unrelated things

## Process

For each unit, try to describe what it does in one sentence without using "and." If you need "and," the unit likely has multiple responsibilities that could be separated.

## Trade-off

Cohesion is a spectrum. Over-splitting creates tiny classes that are hard to navigate. The goal is units where everything inside genuinely belongs together, not units that contain exactly one method.

## Go Deeper

What other units are doing unrelated things? Where are "and" hiding in class or module descriptions?
