# Codex Stage Review Loop

[Simplified Chinese](README.zh-CN.md)

Codex Stage Review Loop is a Codex Skill that turns "please check this before we move on" into a bounded acceptance-review workflow. It helps Codex compare finished work against the original request, inspect real evidence, run available validation, fix verified blocking issues, and stop when a decision belongs to the user.

It is not a claim that a model can catch every defect by reflecting on its own work. The value is more practical: make task closeout explicit, evidence-backed, and low overhead.

## Why It Exists

AI coding agents can complete the main task but still miss the last mile: a skipped check, an unverified assumption, an optional refactor treated as required, or a final answer that sounds more certain than the evidence supports.

This skill provides a small reusable closeout loop for those moments. It is useful after code changes, documentation edits, generated artifacts, local configuration work, or changes to another Codex Skill.

## How It Works

- Locks the review scope to the user's request, touched files, acceptance evidence, and explicit exclusions.
- Checks that each finding, fix, or suggestion still serves the user's original goal instead of creating process-driven extra work.
- Prioritizes inspectable evidence such as diffs, files, tests, build output, logs, rendered UI, or smoke checks before model judgment.
- Runs a fresh-perspective pass to look for missed requirements, secondary bugs, and user-facing gaps.
- Fixes verified blocking issues when the fix is inside scope and low risk.
- Reports optional improvements as suggestions instead of silently expanding the task.
- Stops for user confirmation when the next step is ambiguous, risky, or a product decision.

## Modes

### Lite mode

The default mode for frequent checkpoints. It runs a compact acceptance pass and a fresh-perspective review in the current session, with a small bounded fix loop.

Use it for everyday checkpoints:

```text
Use $stage-review-loop to lightly verify the current work and fix real blocking issues.
```

### Independent reviewer mode

A stronger mode for final confidence or higher-risk work. It prefers a separate reviewer, fresh thread, fresh model surface, or other genuinely independent review surface when available.

If no independent surface is available, the skill should report that it only ran a constrained fresh pass inside the current context.

```text
Use $stage-review-loop in independent reviewer mode to check whether this matches the original request.
```

## Relationship To Codex Review

This skill is not a replacement for Codex's built-in code review or PR review. Use built-in review for focused code-diff findings.

Use this skill when the question is broader than a diff: "Did this task actually meet the request, did we verify it, and are any blocking issues still open?"

They can also be combined: run a code review for diff-level defects, then run this skill as a task-level acceptance pass.

## Limitations

This skill can reduce missed acceptance checks, scope drift, and evidence-free completion claims. It cannot reliably catch problems that require unavailable external facts, missing domain knowledge, absent tests, unclear requirements, or a truly independent reviewer.

The workflow is deliberately lightweight. It should not become a large release process unless the user explicitly asks for a stronger final review.

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
Use $stage-review-loop to verify the current work.
```

## Project Files

- `SKILL.md`: the skill instructions
- `agents/openai.yaml`: Codex UI metadata

## License

MIT
