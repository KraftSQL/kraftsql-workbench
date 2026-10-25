---
name: plan
description: Plan the implementation of a specified KraftSQL feature by marking the necessary code changes with TODO comments, split into a sequence of self-contained steps. Use after the 'specify' step and before implementing.
---

The plan of a feature is written directly into the code as TODO comments, not into a separate plan document.
The result is a change set that a human can review like any other code change, and that guides the implementation later.

## What makes up the plan

- **TODO comments** at each code element that has to change, in the form:
  ```kotlin
  // TODO(plan) step 2: map typeOf<String>() to STRING and typeOf<String?>() to NULLABLE_STRING.
  ```
  - State what to change and, where it isn't obvious, how and why.
  - Mention side effects, e.g. existing tests that need adapting, or existing `TODO`/`FIXME` comments the step resolves.
  - Add comments only. Don't implement anything and don't change code, so everything still compiles as before.
- **Steps**: split the changes into a sequence of self-contained steps that are easy to follow in a later review.
  Each step covers one cohesive change, e.g. one module or connector, together with its tests.
  - **Step 1 is always the reference test** ([kraftsql-core/kraftsql/src/referenceTest/](../../../kraftsql-core/kraftsql/src/referenceTest)):
    implementing the spec'ed reference test cases first, which are expected to fail until the following steps make them pass.
    Note this expectation in the step's TODO.
  - Then the core library, followed by the connectors and their tests.
  - Small features may need only a few steps; don't split artificially.

## Procedure

1. Find the spec commit of the feature, usually the most recent commit named `Specify: <feature>`.
   Ask if it is unclear which commit to plan.
2. Inspect that commit's diff: the spec'ed API, KDoc and unimplemented test cases.
3. Starting from the spec'ed elements, follow only the code they relate to: implementations, usages, callers and the tests that exercise them.
   Don't scan the entire code base.
4. Check the feasibility per connector, i.e. those cloned into this workbench: how each can implement the spec, including efficiency.
5. Write the plan as described above.
6. Report to the user:
   - the steps in order, each with a short summary of its changes
   - what each step is expected to make pass
   - open points, e.g. spec'ed behaviour without test cases, or behaviour a connector cannot implement as spec'ed
