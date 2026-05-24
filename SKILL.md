---
name: stage-review-loop
description: Use after Codex finishes a task, project slice, or skill edit and the user wants a lightweight self-check, blind-style review, bounded fix loop, or optional independent reviewer pass before continuing. Triggers on requests such as review this work, run a self-check loop, blind review, independent reviewer, recursive fix, make sure it matches my requirements, or check the skill/project after implementation.
---

# Stage Review Loop

Use this skill to verify a completed slice of work and fix real issues without turning the review into a large release process.

The goal is not "perfect forever." The goal is: requirements met, known checks pass, no obvious regressions, and no unresolved blocking findings in the current scope.

## Modes

Choose the mode from the user's wording.

- **Lite mode**: default. Use this for frequent checkpoints during a project or skill build. It runs a compact self-check plus one fresh-perspective review pass in the current session.
- **Independent reviewer mode**: use when the user asks for independent review, blind review, fresh reviewer, stronger review, new context, or final confidence. Prefer a separate reviewer or fresh review surface when available. Create a fresh worktree only for high-risk code review, final closeout, or when the user explicitly accepts the extra cost. If no separate reviewer is available, simulate the independent pass by deliberately reviewing only the written requirements, touched files, diffs, and validation output.

If the user does not specify a mode, use lite mode. Do not ask unless the choice materially changes risk or runtime.

## Scope Lock

Before reviewing, define the smallest honest scope:

1. Restate the user request in one short sentence.
2. Identify touched files or artifacts.
3. Identify acceptance evidence: tests, build commands, lint/type checks, manual smoke checks, rendered output, or direct file inspection.
4. Exclude unrelated files, style preferences, and speculative future features.

Do not expand scope just because a possible improvement is visible.

## Review Loop

Default loop limits:

- Lite mode: up to 2 loops.
- Independent reviewer mode: up to 3 loops.
- Stop earlier when a full loop finds no blocking finding.

Blocking findings are:

- required behavior missing
- bug or broken logic
- contradiction with the user's request
- failing validation command
- broken user-facing flow
- unsafe or unauthorized file/config change
- test, documentation, or skill instruction that says one thing while the implementation does another

Non-blocking findings are:

- optional refactors
- naming/style preferences
- extra features
- performance guesses without evidence
- broad architecture changes not needed for the current request

Fix blocking findings immediately when the fix is inside scope and low risk. Record non-blocking findings as suggestions, but do not modify code for them unless the user asked.

## Loop Steps

Run these steps in order.

### 1. Self-acceptance

Check the work against the user's actual request:

- Confirm each requested change is present.
- Inspect the relevant diff or files directly.
- Run the cheapest reliable validation commands available for the project.
- If a command cannot be run, say why and use the next best evidence.
- Look specifically for secondary bugs introduced by the change.
- When reviewing a Codex skill, also check frontmatter triggers, mode-selection rules, loop limits, user-interruption rules, output format, and `agents/openai.yaml` if present.

If a blocking issue is found, fix it and rerun the relevant checks.

### 2. Fresh-perspective review

Review as if seeing the result for the first time:

- Read the user request, changed files, and validation output again.
- Ignore prior confidence and prior review conclusions.
- Treat bot/tool/model comments as candidate findings, not truth.
- Verify each suspected issue against the actual files or command output before accepting it.
- Ask: "Would a user get the result they asked for without needing hidden context?"
- Track accepted findings as evidence-backed items: finding, evidence, decision, and fix.

If using independent reviewer mode and a separate reviewer is available, give it only:

- the user request or current acceptance criteria
- the relevant diff or file list
- validation commands and outputs
- any known constraints or risk boundaries

Do not give it your prior conclusions, suspected fixes, or desired answer unless that is required for the review task.

### 3. Fix and repeat

For each verified blocking finding:

1. Fix only the smallest necessary scope.
2. Re-run the relevant validation.
3. Repeat self-acceptance and fresh-perspective review until the loop limit is reached or no blocking findings remain.

If the loop limit is reached with blocking findings still open, stop and report the remaining issues plainly.

## User Interruption Rules

Stop and ask the user before continuing if any of these are true:

- the requirement can be interpreted in two materially different ways
- the fix would change behavior outside the requested scope
- the fix requires deleting files, overwriting user work, changing credentials, changing network/proxy/auth config, or other risky state
- validation requires paid services, external accounts, private tokens, or long-running jobs
- the best fix is a product decision rather than an engineering correction
- the reviewer finds a serious issue but the fix would be large or uncertain

Ask one concise question in plain language. Include the concrete choice and why it matters.

## Anti-shortcut Rules

Never:

- claim success without checking the relevant files or evidence
- weaken tests or requirements to make validation pass
- silently skip requested behavior
- hide uncertainty behind vague wording
- convert optional suggestions into unrequested rewrites
- keep looping on cosmetic improvements after blocking findings are gone
- describe the result as perfect

## Output Format

Keep the final report short:

```text
Mode: lite | independent reviewer
Loops run: N
Fixed: ...
Verified: ...
Remaining: none | ...
Suggestions: ...
Need user decision: yes/no
```

When speaking to the user, prefer their language. If the user writes in Chinese, explain findings in simple Chinese.
