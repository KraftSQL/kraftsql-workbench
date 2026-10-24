---
name: specify
description: Specify a KraftSQL feature in the code itself, by user-facing API, KDoc, documentation and unimplemented test cases. Detects gaps in the spec and deviations from the existing implementation. Use before implementing a new feature or changing existing behaviour.
---

The specification of a feature is written directly into the code, not into a separate spec document.
The result is a change set that a human can review like any other code change, and that an implementation must then fulfil.

## What makes up the spec

- **API**: user-facing classes, interfaces, functions, properties, etc. are created or altered with their signatures.
  Bodies of new functions are not implemented, but only `TODO()`. Existing implementations stay untouched.
- **KDoc**: each user-facing element describes its behaviour and semantics, including parameters, return values, and failures.
- **Examples**: usage of the feature from a user's point of view, written in the [examples](../../../kraftsql/examples) module.
  Write them before fixing the signatures, to check that the API is Kotlin-idiomatic and type-safe, e.g. needs no casts.
- **Documentation**: READMEs and the [documentation wiki](../../../documentation.wiki) are updated where users would look for the feature.
- **Test cases**: defined only by their descriptive, human-readable name in backticks, never implemented:
  ```kotlin
  @Test
  fun `ConstantData rejects items of different types`() {
      TODO()
  }
  ```
  - Name each test in the style of the existing tests: a full sentence stating the expected behaviour.
  - Unit tests (`src/test/`) specify behaviour independent of any `Engine`, e.g. validation in constructors.
  - The **reference test suite** ([kraftsql/kraftsql/src/referenceTest/](../../../kraftsql/kraftsql/src/referenceTest)) runs a smoke-test for the semantics of KraftSQL's API, which every `Engine` must fulfil.
    Comes on top to the detailed unit tests.
  - Connector repositories (e.g. `kraftsql-bigquery`) get test cases only for their connector-specific extensions or translation details.

The spec does not need to compile.
If it changes signatures, adapting their existing usages is left to the implementation.

## Procedure

1. Understand the feature requested by the user. Ask if its intent is unclear.
2. Study the existing code, KDoc, tests, documentation and [ADRs](../../../.adr) related to the feature.
3. Write the spec as described above.
4. Check for **gaps** in the spec. Typical ones:
   - edge cases: empty data, `null` values, unsupported types, duplicates, ...
   - failure behaviour: what is rejected, when, and with which exception
   - interaction with existing features and the semantics of all `Type`s
   - behaviour that cannot be implemented uniformly by the known connectors, i.e. those cloned into this workbench
   - existing `TODO`/`FIXME` comments touching the feature
5. Check for **deviations** from the existing implementation, where the spec contradicts existing code, KDoc, tests or documentation,
   and for violations of ADRs (e.g. a connector extension changing the behaviour of the core API).
6. Resolve gaps and deviations with the user. Don't invent answers.
   - A confirmed deviation must be specified explicitly: alter the KDoc, documentation and test cases so that they state the new behaviour.
     Existing tests that contradict it are changed into the new, unimplemented test case, not left in place or silently deleted.
   - An unresolved gap is left as a `// TODO(spec): <question>` comment at the related code element.
7. Report to the user:
   - the spec'ed elements and test cases, per repository
   - the deviations from the existing implementation, each one explicitly
   - the open gaps
