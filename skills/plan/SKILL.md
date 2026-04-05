# Plan

Produce a plan in EXACTLY this format. No exploration beyond what's needed to fill it.

## Output format

```markdown
## Goal
One sentence.

## Changes (max 15 lines)
- `file/path.ts`: change description
- `file/path.rs`: change description

## Order of Operations
1. Step one
2. Step two
(max 8 steps)

## Risks
- Risk one
- Risk two
(max 3 bullets)
```

## Rules

1. Total planning time: under 3 minutes. Read only the files you need.
2. Do NOT write code. Do NOT start implementing.
3. Save the plan to `PLAN.md` in the repo root.
4. If a previous `PLAN.md` exists, read it first and ask whether to replace or append.
5. After writing the plan, say: "Plan written to PLAN.md. Run /execute to implement, or /ship to implement and commit."
