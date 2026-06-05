---
name: pr-review
description: Review pull requests, local diffs, completed ticket code, or a colleague's code with a practical senior-engineering lens. Use when Codex is asked to review PRs or code changes for readability, maintainability, elegance, simplification opportunities, hacky patterns, best-practice issues, React pitfalls such as unnecessary effects or over-memoization, comment quality, and clarification questions for the author.
---

# PR Review

## Review Posture

Review like a thoughtful teammate who wants the code to be easier to understand, change, and trust. Prioritize issues that affect behavior, maintainability, clarity, or future risk. Avoid performative nitpicks and avoid asking for abstraction unless it clearly reduces complexity or matches the local codebase.

Assume the author may have context you do not. When something looks odd but may be intentional, raise it as a question instead of a finding.

## Workflow

1. Gather context:
   - Inspect the diff, surrounding code, relevant tests, and local patterns before judging.
   - Identify the feature intent from the PR title/body, ticket notes, commit message, or user prompt when available.
   - If reviewing a local working tree, use `git status`, `git diff`, and targeted file reads.
   - If reviewing a GitHub PR, use available GitHub tools or `gh pr view` / `gh pr diff` when appropriate.

2. Review for correctness first:
   - Look for behavior regressions, broken edge cases, data integrity issues, authorization gaps, stale state, race conditions, unhandled async failures, and missing tests.
   - Treat correctness and security issues as findings even when the code is otherwise clean.

3. Review for readability and maintainability:
   - Ask whether the code is easy to scan, locally understandable, and named in a way that explains intent.
   - Prefer simple direct code over cleverness.
   - Flag duplication only when it creates real maintenance risk or obscures intent.
   - Suggest simplification when the current design is more complex than the behavior requires.

4. Review for best practices without cargo culting:
   - In React, check for unnecessary `useEffect`, derived state stored in state, stale closures, overuse of `useMemo` / `useCallback`, unstable dependencies, and effects doing work that could happen during render or in event handlers.
   - In TypeScript, check for avoidable `any`, unsafe casts, unclear generics, weak null handling, and type shapes that hide real invariants.
   - In API/database code, check validation boundaries, transaction safety, permission checks, idempotency, and error behavior.
   - Match the repo's existing conventions unless there is a concrete reason to diverge.

5. Review comments:
   - Flag comments that restate obvious code, drift from behavior, or excuse confusing implementation.
   - Suggest comments where intent, business rule, ordering constraint, migration caveat, or non-obvious tradeoff would otherwise be hard to recover.
   - Prefer improving names or structure over adding comments when that would make the intent self-evident.

6. Produce clarification questions:
   - Ask concise questions for decisions that may be intentional but are not explained by the code or PR context.
   - Focus questions on product behavior, data assumptions, performance tradeoffs, rollout risk, edge cases, and why a particular design was chosen.
   - Do not disguise required fixes as questions. If something is demonstrably wrong, state it as a finding.

## Output Shape

Lead with findings, ordered by severity. Use a numbered list for findings so each issue has a stable reference the user can reply to, even when multiple findings share the same priority. Use file and line references when possible.

Use this structure:

```markdown
**Findings**
1. [P1] Title - `path/to/file.ts:42`
   Explain the problem, why it matters, and the smallest useful fix.
2. [P2] Another title - `path/to/other-file.ts:84`
   Explain the problem, why it matters, and the smallest useful fix.

**Questions**
- What should happen when ...?

**Notes**
- Optional maintainability or style observations that are worth considering but not blocking.

**Summary**
Brief overall judgment of the change.
```

If there are no meaningful findings, say that clearly and mention any residual test gap or uncertainty.

## Severity

- `P0`: Breaks core behavior, creates critical security/data loss risk, or blocks release.
- `P1`: Likely bug, important missing guard, or maintainability issue with near-term risk.
- `P2`: Real improvement worth addressing, but not urgent.
- `P3`: Minor polish; include sparingly and only when useful.

## Inline Comment Style

When the user asks for review comments, write comments as if posting on a PR:

- Be direct but kind.
- Explain the impact, not just the preference.
- Suggest a concrete change when possible.
- Keep comments short enough to be useful in a code-review thread.

Avoid:

- Personal judgment about the author.
- Vague comments such as "clean this up".
- Nitpicks that do not improve clarity, behavior, or consistency.
