# KraftSQL's own Kotlin API, implemented by connectors per SQL engine

## Decision

**Kraft**SQL defines its own, Kotlin-idiomatic API and semantics to work with structured, tabular data,
hiding the quirks of individual SQL engines.

- The core library (`kraftsql`) defines this API: abstractions like `Data`, `Column` and `Type` and the operations on them, with semantics defined by **Kraft**SQL.
- An `Engine` executes this API. Each SQL engine gets a connector, which implements `Engine` and translates **Kraft**SQL's API into the engine's native SQL and types.
  - Connectors may offer extensions for features specific to their SQL engine, as long as these are pure additions and do not change the behaviour of the core API.
- The core library ships a reference test suite (`referenceTest`), which every connector must pass to prove it implements **Kraft**SQL's semantics.

### Explanation

A fist approach modelled SQL in Kotlin as precisely as possible, including vendor-specific extensions and quirks.
This failed mainly because SQL's semantics are not Kotlin's:
the type systems of SQL engines diverge from Kotlin's and from each other, and each engine applies its own automatic type conversions.
Mirroring this precisely in Kotlin, while keeping its static typing and type-safety, was hardly implementable.

With **Kraft**SQL defining its own API and semantics, the types and their behaviour are defined once, in Kotlin terms.
Mapping them to the native types and conversions of an SQL engine happens behind the scenes by the connector.
It does not complicate the API, which would otherwise have to allow an abstraction over all SQL engines and a restriction to each specific engine at the same time.

As an extra benefit, user code becomes more portable between engines, especially to a single `kraftsql-local` engine for unit-testing,
although portability is not guaranteed though, as soon as user code uses connector-specific extensions.

Accepted disadvantages:
- More complex translation effort from the non-SQL **Kraft**SQL API to SQL engines.
- Connectors must work around engine differences, which may not always be possible efficiently.

### Comparison of alternatives

|                                      | precise SQL model (+ DSL)              | *own API, implemented by connectors*   |
|--------------------------------------|----------------------------------------|----------------------------------------|
| type-safety with static Kotlin types | :-1: divergent SQL type systems and conversions hardly expressible | :+1: through **Kraft**SQL's types    |
| Kotlin-idiomatic API                 | :-1: SQL-shaped, needs extra DSL       | :+1: designed for Kotlin               |
| SQL-idiomatic API                    | :+1: SQL-shaped, well-known            | :-1: proprietary API for users to learn |
| full access to engine specifics      | :+1: modelled explicitly               | :shrug: via connector-specific extensions |
| implementation effort for SQL generation | :+1: SQL structure already in SQL-idiomatic API | :-1: translation from Kotlin-idiomatic API to SQL |
| implementation effort for simulators | :-1: simulation of all quirks necessary | :+1: single API and semantic to simulate |
| portability of the user code         | :shrug: with medium effort by changing to another connector API | :+1: connector change easily possible |
