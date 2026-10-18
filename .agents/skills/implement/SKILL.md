---
name: implement
description: Implement a planned KraftSQL feature by working through the `TODO(plan)` steps in the code, one commit per step. Can implement the entire plan or only selected steps. Use after the 'plan' step.
---

The implementation follows the plan, which is written directly into the code as `// TODO(plan) step N: ...` comments.
Each step results in its own commit, so that a human can review, test and commit the steps one by one.

## What makes up a step

- **Implementation** of everything the step's `TODO(plan)` comments describe, including the tests they mention.
  - Write code that reads like the surrounding code: naming, idiom, comment density and test style.
  - Resolve the existing `TODO`/`FIXME` comments the step names, and remove or shorten them accordingly.
  - Don't anticipate later steps. Their `TODO(plan)` comments stay in place.
- **Removal** of the step's `TODO(plan)` comments. If the step changes what a later step has to do, update that step's comment.
- **Tests** of the affected modules pass, except for those the plan expects to fail until a later step,
  e.g. the reference tests of step 1.
- **One commit** per step, with a message describing the change in the style of the existing commit messages,
  e.g. `Implement support for Strings in the type-system`. Don't mention the step number.

## Procedure

1. Find the plan, usually the most recent commit named `Plan: <feature>`, and the `TODO(plan)` comments left in the code,
   e.g. with `git grep -n "TODO(plan)"` in each repository of the workbench.
   Ask if it is unclear which plan to implement.
2. Determine the steps to implement: the ones the user selected, or else all remaining steps in order.
   A step whose predecessors are not implemented yet is only implemented if the user asks for it explicitly.
3. For each step:
   1. Inspect its `TODO(plan) step N` comments, and the code and tests they relate to.
   2. Implement it as described above.
   3. Run the tests of the affected modules, e.g. `./gradlew :kraftsql:test :kraftsql-local:integrationTest`,
      and check the results against what the plan expects.
   4. Commit it.

   Stop and ask the user instead of committing, if
   - the plan turns out to be wrong or incomplete, e.g. a test the step should make pass still fails for a reason outside the step,
   - an unplanned change is necessary, or
   - the tests cannot be run.

   If the user requests a deviation from the plan, e.g. pulling part of a later step forward, implement it as its own commit
   and adapt the affected `TODO(plan)` comments.
4. Report to the user, per step:
   - the commit and a short summary of its changes
   - the test results, including the tests still expected to fail
   - deviations from the plan, and remaining `TODO(plan)` comments
