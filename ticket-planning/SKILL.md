---
name: ticket-planning
description: Understand and plan software tickets before implementation. Use when Codex is given ticket context, ClickUp links, Figma links, existing plan docs, PR links, or extra caveats and is asked to understand scope, identify gaps, ask clarifying questions, draft or refine a high-level plan, draw implementation or user-flow diagrams, or create a Markdown handoff plan for a fresh implementation chat. Use especially when the user says not to code yet, wants planning first, or wants repeated clarification before starting.
---

# Ticket Planning

## Posture

Treat the ticket as a discovery and planning task, not an implementation task. Do not edit production code unless the user explicitly asks to start implementation.

Act like a careful teammate helping the user de-risk the work before context is spent on coding. Separate what is confirmed from what is assumed, what conflicts, and what still needs a decision.

## Inputs To Gather

Inspect the sources the user provides:

- Ticket text, ClickUp link, acceptance criteria, BDD scenarios, screenshots, comments, and attachments.
- Figma links, selected nodes, design states, and copy differences.
- Extra user caveats, especially notes that generated ticket text may be wrong.
- Existing docs, plans, PRs, branches, local diffs, and related code paths.
- Product/team clarifications already provided by the user.

When tool access is available, fetch ClickUp/Figma/GitHub context directly. When access fails or is unavailable, proceed from the pasted context and say what could not be verified.

## Workflow

### 1. Discovery Report

First understand the ticket and report back before planning deeply.

Include:

- **Goal**: what user/product outcome the ticket appears to request.
- **Confirmed scope**: work that is clearly required.
- **Likely out of scope**: work that should not be done unless confirmed.
- **Existing state**: what appears already implemented or reusable.
- **Conflicts**: differences between ticket, Figma, existing code, and the user's extra context.
- **Assumptions**: things inferred but not proven.
- **Early implementation shape**: a rough map of likely touched areas, kept lightweight.

Keep the early implementation shape intentionally rough. Use it to expose gaps, not to lock into a plan.

### 2. Clarification Loop

Ask questions before drafting the full plan when uncertainty could change scope, architecture, data flow, UI behavior, or testing.

Bucket questions:

- **Questions for you**: decisions the user can answer from project context or preference.
- **Questions for product/team lead**: behavior, copy, edge cases, permissions, or rollout decisions that need ownership.
- **Questions I can investigate**: items likely answerable by reading code, Figma, ClickUp, or PR history.

Prefer fewer, high-value questions. Re-run discovery after the user answers and call out what changed.

Continue the loop until the user indicates the scope is sufficiently clear or asks for the plan.

### 3. High-Level Plan

When asked to plan, produce a plan that is concrete enough for implementation but not overloaded with speculative detail.

Include:

- Implementation phases or milestones.
- Likely files, packages, components, API endpoints, schemas, or services to inspect or change.
- Data flow, UI flow, permission flow, or state flow when relevant.
- Dependencies on unresolved questions.
- Risks and edge cases.
- Test and verification strategy.
- Suggested order of work.

If a flow, state machine, or system interaction is involved, include a Mermaid diagram. Use simple diagrams that clarify sequencing or ownership.

### 4. Plan Review Loop

Invite the user to review the plan before implementation. When the user gives feedback, revise the plan and explicitly list:

- Decisions incorporated.
- Scope changes.
- Remaining open questions.
- Anything intentionally deferred.

Do not treat the first plan as final if the user is still challenging assumptions.

### 5. Markdown Handoff

When the user asks to dump the plan to a Markdown file, create or update a concise handoff document that a fresh chat can use to implement.

Use this structure:

```markdown
# <Ticket / Feature Name> Plan

## Context
- Ticket:
- Figma:
- Related docs / PRs:
- User notes:

## Goal

## Confirmed Decisions

## Out Of Scope

## Open Questions

## Implementation Plan

## TODO

## Files / Areas To Inspect

## Risks And Edge Cases

## Verification

## Notes For Fresh Implementation Chat
```

Keep the handoff practical. Include enough context to avoid re-discovery, but avoid dumping the whole conversation.

## Response Style

- State uncertainty plainly.
- Do not hide disagreements between sources.
- Prefer "I think this means..." plus evidence over confident guesses.
- Keep product questions separate from engineering questions.
- Ask before implementing unless the user clearly says to start.
- If the user asks for only understanding, stop at understanding and questions.
