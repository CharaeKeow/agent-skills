---
name: ticket-planning-context
description: Understand and plan software tickets by combining all available context, resolving gaps through discussion, and producing an implementation-ready plan. Use when planning before coding, whether the source is a prototype, Figma, bug report, PR, refactor, or incomplete product requirement.
user-invocable: true
---

# Ticket Planning Context

Plan the work before implementation.

Treat the ticket, prototype, Figma, existing code, previous discussions, PRs, and
user guidance as sources of evidence. Do not assume that any particular source
exists before inspecting it.

Do not edit production code unless the user explicitly asks to begin
implementation.

## Context discovery

Inspect the sources that are available and relevant:

- ticket text, acceptance criteria, comments, and attachments;
- product requirements or PM-generated descriptions;
- existing plans and related documentation;
- prototype behavior and current implementation;
- Figma or other design references;
- related PRs, branches, and local diffs;
- prior decisions and unresolved discussions;
- relevant tests and runtime call paths.

When sources disagree, surface the disagreement instead of silently choosing one.

When a prototype exists, inspect what it already demonstrates. When no prototype
exists, plan from the supplied requirements, design sources, existing product
patterns, and codebase evidence.

## Requirement impact check

Treat the PRD as a claim to verify, not the source of truth. PRDs may be
AI-written or written after a prototype to describe it, so a stated behavior
may never have been weighed against the rest of the system.

For each entity, field, value set, status, or event the ticket creates or
changes:

1. Find every consumer outside the ticket: forms, workflow blocks, triggers,
   reports, exports, notifications, other apps and services.
2. Check that each consumer accepts every value the requirement allows.
3. For each mismatch, state what happens: a loud error, or a silent wrong
   result. Silent ones matter most.
4. Raise each mismatch as a product question with code references. Do not
   resolve it by narrowing or widening scope yourself.

Example: a new "assignee" field allowed admins per the PRD, but the workflow
steps that read it only handle regular users. An admin value made those steps
silently fall back to a default assignee instead of raising an error.

## Discovery report

Begin with a compact discovery report:

- **Goal** — the intended user or product outcome.
- **Available context** — which sources were found and inspected.
- **Confirmed scope** — behavior clearly required by the evidence.
- **Existing state** — what already works, exists, or can be reused.
- **Gaps and conflicts** — contradictions or missing behavior.
- **Impact on other modules** — consumers of new or changed data, and any
  mismatch with what the requirement allows.
- **Assumptions** — inferred behavior that is not yet confirmed.
- **Likely implementation shape** — a rough map of affected areas.
- **Deferred scope** — useful work that should not block the ticket.
- **Questions** — only decisions that could change scope or implementation.

Separate product decisions from engineering decisions. Mark recommendations as
recommendations, not approved scope.

## Discussion loop

Planning is collaborative and iterative.

Ask high-value questions when uncertainty could change:

- user-visible behavior;
- data or state ownership;
- permissions;
- API or persistence design;
- migration strategy;
- testing expectations;
- rollout or compatibility behavior.

Investigate directly when the answer can be established from code, tests,
documentation, or history.

After the user responds, revise the understanding and explicitly state:

- decisions incorporated;
- scope added or removed;
- assumptions resolved;
- remaining questions;
- intentionally deferred work.

Do not jump to implementation merely because the likely files are known.

## Choosing the implementation shape

Choose the planning shape that fits the work.

Use vertical slices when the ticket contains observable end-to-end behavior:

```text
user action
  → UI or entry-point state change
  → API or persistence path
  → success and failure behavior
  → focused verification
```

Use a smaller targeted plan for isolated bug fixes, refactors, migrations, or
documentation changes when vertical slices would add ceremony without improving
clarity.

Vertical slices are a preferred option, not a mandatory format.

## State and invariants (only when it applies)

Use this when the ticket has a lifecycle, retries, webhooks or callbacks,
concurrent writers, or money/ledger rules. Skip it otherwise.

Before writing slices, agree with the user on:

- **States, events, transitions** — a small table, including the events that
  arrive late, twice, or out of order.
- **Invariants** — 3–5 rules that must hold after any event sequence
  (e.g. "a payment is confirmed at most once").
- **Where they're enforced** — a pure transition function, a DB constraint,
  or a conditional update. Name which one for each invariant.
- **How they're tested** — example tests by default; a property test
  (random event sequences, check invariants after each step) only when the
  orderings are too many to list by hand.

Model state as a discriminated union with a pure transition function. Don't
add a state-machine library, a property-testing dependency, or a formal model
without the user's approval.

## Plan output

When the user asks for the plan, use the structure that best fits the work. For
feature work, prefer:

```markdown
# <Feature> Plan

## Goal

## Confirmed decisions

## State model and invariants

## Existing state and relevant call paths

## Scope

## Implementation slices

### Slice 1 — <observable outcome>

- User behavior:
- State and data flow:
- Implementation areas:
- Success behavior:
- Failure behavior:
- Verification:

## Out of scope

## Open questions

## Risks and edge cases

## Suggested implementation order

## Verification strategy
```

Include "State model and invariants" only when the State and invariants
section applied; omit it otherwise.

For bug fixes or refactors, use a focused structure instead of forcing slices.

Include a Mermaid diagram when it materially clarifies a state, ownership, or
request flow.

## Handoff

Only create or update a Markdown handoff file when the user explicitly asks for
one.

Keep the handoff concise and include context references, confirmed decisions,
scope, implementation shape, unresolved questions, risks, and verification steps
needed by a fresh implementation task.

Do not begin implementation until the user indicates that the scope and plan are
ready.
