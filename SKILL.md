---
name: pr-presubmit
version: 1.0.0
description: >
  Use this skill to run a structured pre-submit checklist on a change before it goes out
  for review, helping the author catch everything catchable before a reviewer ever looks.
  Activate when a user is about to open or push a pull request, is getting ready to
  submit or merge changes, wants to know whether their PR is ready, asks for a
  pre-submit or pre-flight check, or describes a change and asks whether anything is
  missing before requesting review.
  Do not use this skill for writing or improving a PR title and description (use
  pr-best-practices for that), reviewing someone else's code (use pr-review for that),
  or running automated tests or linters directly on behalf of the user.
allowed-tools: []
---

# PR Pre-Submit Checklist

The goal is to catch what's catchable before a reviewer ever looks at your code. A reviewer's time is expensive — the more you can resolve on your own, the faster the review goes and the better the signal-to-noise ratio of their comments.

Work through this with the user. Ask what the change does if you don't know — the checklist items only make sense in context.

---

## 1. Code Correctness & Testing

**Does it build and pass?**
Ask the user: does the code compile locally with zero warnings? Do all tests pass — unit, integration, and end-to-end? If they haven't run the full suite, flag that now. Automated tests are the minimum bar, not a nice-to-have.

**Self-review the diff.**
Encourage the user to walk through their own diff in a Git GUI or GitHub's diff view *before* requesting review — not the editor, where familiarity hides things. Specifically look for:
- Debug logs, `console.log`, `print()` left in
- Commented-out code or orphaned files
- TODO/FIXME introduced and left unresolved
- Logic that's half-done or placeholder

**Edge cases covered?**
Think through failure modes: null/empty inputs, network timeouts, auth failures, negative numbers, empty collections, concurrent access. If any of these apply to the change, is there test coverage or at least defensive handling?

---

## 2. Formatting & Standards

**Linting clean?**
Static analysis should pass with no new warnings. If the team has a lint step in CI, run it locally first so reviewers don't see a failing check as their first impression.

**Naming is communication.**
Variable names, method names, class names should be self-explanatory to someone who wasn't in the room when you wrote the code. If a name needs a comment to explain it, the name probably needs work. This is especially worth checking on anything new you've introduced.

**No hardcoded secrets.**
Scan the diff for API keys, tokens, passwords, internal URLs, or any value that varies by environment. These belong in environment variables or a secrets manager — not in the diff. This is a security issue and often a compliance issue; it's one of the easiest things to miss in the flow of writing code.

---

## 3. PR Hygiene & Context

**Scope: one logical thing.**
The PR should map to a single ticket or feature. If reviewing the diff feels like reading two different stories, it probably needs to be split. Mixed scope makes reviewers context-switch and makes rollback harder. If the user's PR is doing multiple things, flag it and suggest how to split.

**Description: what changed and why.**
If the team has a PR template, it should be filled out. At minimum the description should answer:
- *What* changed (the implementation)
- *Why* it changed (the motivation — the bug, the user need, the business reason)

A title that says "fix bug" and an empty body forces the reviewer to reverse-engineer intent from the diff. That's friction you can avoid.

**Visual proof for UI changes.**
Any user-facing front-end change should include a before/after screenshot or GIF. This lets reviewers verify the visual result without checking out the branch. If the change is pure logic with no visible effect, skip this — don't manufacture a screenshot for something that doesn't need it.

---

## How to run this checklist

Go through each section with the user. For each item:
- If it clearly applies to their change, ask whether they've done it
- If it doesn't apply (e.g., no UI change means no screenshot needed), skip it cleanly
- If they haven't done something that matters, help them fix it or note it as a remaining action

End with a summary: what's clear to merge, what still needs doing. Keep it concrete — "run the integration tests, then you're good" beats a wall of caveats.
