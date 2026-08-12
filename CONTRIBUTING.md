# Contributing to PrepSchool

Thank you for contributing to PrepSchool. Keep changes reviewable, preserve the product and architecture constraints in `AGENTS.md`, and read the product source-of-truth documents before architecture or implementation work.

## Contribution workflow

Use this workflow for code, documentation, and research:

```text
Issue / ticket
→ feature branch
→ implementation or research
→ tests / validation
→ human pre-commit review for agent-authored work
→ commit
→ Draft PR
→ review comments
→ revisions
→ CI
→ human merge
```

1. Start from an issue or ticket with a defined scope and acceptance criteria.
2. Create a focused feature, fix, research, documentation, or chore branch. Do not push directly to `main`.
3. Implement the change or record the research without silently replacing existing architecture.
4. Run tests and validation appropriate to the files and behavior changed. Record what was run and any limitations.
5. Before committing agent-authored work, the responsible human contributor must inspect the exact diff, validate the result, and accept responsibility for submitting it.
6. Create a focused commit authored by that human contributor.
7. Open a Draft PR early enough to make scope, assumptions, and unresolved questions visible.
8. Address review comments with additional validation where needed.
9. Wait for required CI checks once CI exists. Do not represent unrun checks as passing.
10. A human reviewer or maintainer makes the final merge decision.

## Pull requests

- Keep each PR focused on one coherent ticket or outcome.
- Explain changes, non-goals, validation, security/privacy impact, architecture impact, and the areas needing review.
- Separate unrelated refactors from behavioral or architectural changes.
- Add tests for behavioral changes.
- Preserve existing work and call out unresolved issues rather than hiding them.
- Significant architecture changes require prior research and/or ADR review. Research is informational until an architecture decision is explicitly accepted.

## Commit authorship and AI-assisted work

Commits are authored by the responsible human contributor who reviews and submits the change. AI agents are development tools and never receive commit authorship, `Co-authored-by` attribution, or other agent authorship metadata.

Agents must stop before making substantive commits unless a human explicitly authorizes the commit. Authorization to edit, test, stage, or prepare a commit does not by itself authorize committing or merging. Humans remain the final merge authority.

## Security and privacy

Do not include credentials, tokens, private candidate data, recordings, protected evaluator material, reference solutions, or harness authentication in commits, issues, logs, or PR descriptions. Follow `SECURITY.md` for sensitive vulnerability reports.

Raw camera and audio data remains local and ephemeral by default unless recording is explicitly enabled. Reaction signals must remain distinct from scoring evidence and must not directly alter interview scores.
