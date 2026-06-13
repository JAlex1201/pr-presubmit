# pr-presubmit

A Claude Code skill for running a structured pre-submit checklist on a change before it goes out for review. It helps the author catch everything catchable before a reviewer ever looks, so the review focuses on substance rather than basics.

## What this skill does

This skill walks you through three areas before you open a pull request:

1. **Code correctness and testing** - Confirms the build passes, all tests run, the diff has been self-reviewed, and edge cases are handled.
2. **Formatting and standards** - Checks that linting is clean, names communicate intent, and no secrets or API keys have been committed.
3. **PR hygiene and context** - Verifies the change is scoped to one logical thing, the description explains what changed and why, and UI changes include screenshots.

It ends with a concrete summary: what is ready and what still needs doing.

## Trigger phrases

Claude activates this skill when you say things like:

- "Is my PR ready to submit?"
- "Run a pre-submit check"
- "Pre-flight check before I open this PR"
- "Am I missing anything before requesting review?"
- "Check this before I push"
- "What should I verify before opening this merge request?"
- "Is there anything wrong with this change before I submit it?"

## When to use it

Use this skill any time you are:

- About to open or push a pull request
- Getting ready to request review
- Unsure whether your change is ready
- Wanting a checklist to work through before submitting

## File structure

```
SKILL.md    # Full checklist instructions and how to run them
```

## The checklist

### 1. Code correctness and testing

- Code compiles with zero warnings
- All tests pass: unit, integration, and end-to-end
- Diff self-reviewed outside the editor (debug logs, commented-out code, unresolved TODOs removed)
- Edge cases covered: null inputs, timeouts, auth failures, empty collections, concurrent access

### 2. Formatting and standards

- Linting passes with no new warnings
- Variable, method, and class names are self-explanatory without a comment
- No hardcoded secrets, API keys, tokens, passwords, or environment-specific URLs in the diff

### 3. PR hygiene and context

- Change maps to a single ticket or feature (not two unrelated stories mixed together)
- Description answers: what changed and why it changed
- UI changes include a before/after screenshot or GIF

## How the skill runs

Claude works through each section with you. For each item:

- If it applies to your change, Claude asks whether you have done it.
- If it does not apply (no UI change means no screenshot needed), Claude skips it cleanly.
- If something important is missing, Claude helps you fix it or marks it as a remaining action.

The session ends with a plain summary: what is clear to merge and what still needs doing. "Run the integration tests, then you are good" rather than a wall of caveats.

## Security notes

This skill requires no shell execution or filesystem write permissions. It works through conversation - asking you questions and reviewing the context you provide. No tools beyond read access are needed.

If the user asks Claude to run tests or linters, those commands are executed via the user's normal shell access, not granted by this skill.

## Installation

### Via Claude Code

1. Download `pr-presubmit.skill` (the packaged version of this repo).
2. In Claude Code, run:
   ```
   /install-skill pr-presubmit.skill
   ```
3. The skill is now available in your session.

### Manual setup

Clone this repo and point your Claude Code project at it, or copy `SKILL.md` into your project's `.claude/skills/` directory.

## Guidelines for contributors

- **One skill, one purpose.** This skill focuses on the pre-submit checklist. Do not expand it to cover PR writing, code review, or post-merge work.
- **Specific trigger words.** Any changes to the description frontmatter should include clear trigger phrases so Claude can activate this skill accurately.
- **Progressive disclosure.** Keep `SKILL.md` focused. If additional examples or guidance are added, place them in a `references/` directory rather than growing the main file.
- **Concrete actions.** Each checklist item should describe a specific action the author can take, not abstract advice.
- **Minimal permissions.** Do not add bash or shell execution to this skill. It does not need them.

## Related skills

- [pr-best-practices](https://github.com/julianbesonen/pr-best-practices) - Author strong pull request titles and descriptions
- [pr-review](https://github.com/julianbesonen/pr-review) - Write thoughtful, collaborative code review comments
