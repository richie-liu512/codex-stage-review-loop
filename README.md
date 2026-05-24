# Codex Stage Review Loop

A lightweight Codex skill for checking work after a task, project slice, or skill edit is finished. It helps Codex verify requirements, fix real issues, run a bounded review loop, and stop for user confirmation when a decision is needed.

This is useful when you do not want a heavyweight final PR review, but still want a repeatable checkpoint after each meaningful piece of work.

## What It Does

- Runs a compact self-acceptance pass against the user's request.
- Performs a fresh-perspective review to catch missed logic gaps.
- Supports an optional independent reviewer mode for stronger blind-style review.
- Fixes verified blocking issues inside the current scope.
- Keeps optional refactors and cosmetic ideas as suggestions instead of silently expanding the task.
- Stops when the fix requires a user decision, risky state changes, or a broader product choice.

## Modes

### Lite mode

Default mode for frequent checkpoints. It runs up to two review loops in the current session.

Use this for:

- checking a small project slice
- reviewing a newly edited skill
- catching obvious bugs before moving on
- avoiding large review overhead during active development

Example:

```text
Use $stage-review-loop to lightly check the current changes and fix real issues.
```

### Independent reviewer mode

Stronger mode for blind-style review. It prefers a separate reviewer or fresh review surface when available, and falls back to a constrained fresh-perspective pass when not.

Use this for:

- final confidence before publishing
- reviewing a larger or riskier change
- checking whether the result matches the original request without relying on prior assumptions

Example:

```text
Use $stage-review-loop in independent reviewer mode to blind-review this skill.
```

## Install

Copy the skill folder into your Codex skills directory:

```text
stage-review-loop/
  SKILL.md
  agents/
    openai.yaml
```

Then invoke it by name:

```text
Use $stage-review-loop to check the current work.
```

## Design Notes

The skill intentionally avoids promising "perfect" results. Instead, it uses bounded loops and evidence-backed findings:

- blocking findings are fixed when they are verified and inside scope
- non-blocking suggestions are reported without broad rewrites
- loop limits prevent endless cosmetic churn
- user confirmation is required for risky or ambiguous decisions

## Project Files

- `SKILL.md`: the skill instructions
- `agents/openai.yaml`: Codex UI metadata

## License

MIT
