---
name: parse-dont-validate
description: Parse, don't validate — use when designing types or schemas for data crossing a boundary (API input, JSON, env vars, user input), when writing validation or input-checking logic, or when reviewing code that re-checks data already checked elsewhere.
---

# Parse, Don't Validate

A validator checks a property and throws the knowledge away; a parser checks the same property and **keeps the proof** in its return type. Every check lands on one side of that line:

```ts
validateNonEmpty(xs: T[]): void        // caller learns nothing; downstream must trust or re-check
parseNonEmpty(xs: T[]): NonEmpty<T>    // the proof now travels with the value
```

Parsing, broadly: any function from less-structured input to more-structured output. `JSON.parse`, `zodSchema.parse`, `asUserId`, `toNonEmpty` — input need not be text. A value of the refined type is the original value **plus a proof** that the property holds; the type checker then enforces downstream what the runtime checked once.

## Rules

Apply these to every point where code accepts data it did not construct:

1. **Make illegal states unrepresentable.** Choose the most precise type the data admits; when the current encoding can't express a property you rely on, refactor the type rather than guarding around it.
2. **Push the proof to the boundary — but no further.** Convert raw input to the refined representation at the edge (route handler, form submit, config load), before any logic touches it. Core code then takes the refined type as its argument: written against the representation you wish you had, total over its input.
3. **Let datatypes inform code.** Reach for a discriminated union where control flow follows the data's shape; a bag of boolean flags on a record is the signal to do so.
4. **Treat proof-discarding checks with suspicion.** A function that only checks and returns `void`/`boolean` is a validator; return the refined type instead, or a type predicate (`xs is NonEmpty<T>`) at minimum.
5. **Multiple passes are fine.** Stratification means *parse, then execute* — it permits context-sensitive parsing where one part of the input guides parsing the rest.
6. **Normalize.** Duplicated or denormalized data makes the illegal state "these two copies disagree" trivially representable; where denormalization is required, seal it behind an abstraction boundary that maintains the invariant.
7. **Smart constructors when types run out.** For properties the type system can't express structurally (an int in range, a valid email), use a branded/opaque type whose only constructor is the parser — the validator now *looks like* a parser from the outside.

## Shotgun parsing

The failure mode: checks scattered through processing code, so the program acts on partially-checked data, and after a mid-stream failure its state is unknowable — some effects done, some not, and no way to know what was ever checked. Stratify instead: a parse phase that can reject, then an execute phase that cannot fail on input.

## Done when

Every acceptance point of external or less-structured data in the code under work is accounted for: each one either parses into a refined type consumed downstream, or is explicitly flagged (in review) as a shotgun-parsing site with the refined type it should produce.
