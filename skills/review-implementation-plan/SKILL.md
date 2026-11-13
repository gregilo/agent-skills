---
name: review-implementation-plan
description: Review an implementation plan against its stated goals, the current codebase, and available documentation. Use when the user asks to validate, critique, or stress-test a proposed implementation plan before coding begins.
disable-model-invocation: true
---

# Review Implementation Plan

Review the implementation plan supplied with the invocation. The argument may be
the plan itself, a local file path, or a URL. If the plan is in the surrounding
conversation rather than the invocation, use that plan.

This is a read-only review. Inspect code, history, tests, documentation, issues,
and merge requests when useful, but do not edit files or mutate local or remote
state.

## Establish the Problem

Before judging the proposed solution:

- Identify the root problem, desired outcome, constraints, and explicit
  non-goals claimed by the plan.
- Verify material claims against the current implementation and authoritative
  documentation when those sources are available. Do not treat the plan's
  explanation of current behavior as established fact.
- Distinguish facts supported by evidence from reasonable inferences and from
  unresolved assumptions.

If the plan omits its motivation, infer it only when the repository or linked
context makes it clear. Otherwise identify the missing context rather than
inventing a rationale.

## Evaluate the Plan

Assess whether the plan:

- addresses the verified root problem rather than only a symptom;
- is technically feasible in the affected architecture and respects repository
  conventions;
- identifies all materially affected callers, contracts, data flows, packages,
  generated artifacts, and documentation;
- accounts for compatibility, migrations, failure modes, edge cases, security,
  performance, observability, rollout, and rollback where they are relevant;
- proposes tests that demonstrate the intended behavior and guard against the
  important regressions;
- orders dependent work correctly and gives an implementer enough specificity
  without prescribing unsupported details.

Raise an alternative only when it materially improves correctness, simplicity,
risk, or alignment with the goal. Explain the tradeoff; do not list alternatives
merely because they are possible.

## Missing Context

Gather accessible context yourself before asking the user for it. If evidence
needed for a confident review lives in an unavailable codebase, private document,
decision record, issue, or operational system, name the exact artifact or question
needed and explain which conclusion depends on it. Continue reviewing everything
that does not depend on that information.

## Output

Lead with an overall assessment of whether the plan is sound and ready to
implement.

Then provide only the sections that have useful content:

- **What the plan is solving**: the verified problem and intended outcome.
- **Findings**: concrete flaws, gaps, or unsupported assumptions, ordered by
  impact. For each, cite the relevant plan step and supporting source location
  when available, explain the consequence, and recommend a correction.
- **Better alternatives**: materially stronger approaches and their tradeoffs.
- **Missing information**: specific context the user needs to supply.
- **What looks right**: important choices that are well-supported, especially
  when that helps preserve them during revision.

Label uncertainty explicitly. If there are no substantive findings, say the plan
looks ready rather than manufacturing concerns.
