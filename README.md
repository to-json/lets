# plan-harness

Claude Code skills for structured planning and governed execution.

These aren't really for you, unless I sent it to you personally.

## Skills

### `/plan`

Quick tactical plan. Reads the minimum necessary, outputs a fixed-format plan (goal, changes, order of operations, risks) to `PLAN.md`. Under 3 minutes, no code written.

### `/lets`

Deliberative planning for bigger work. Interviews you to understand intent, drafts a structured plan with streams and sequences, then runs bimodal review (general + adversarial) to stress-test it before any code exists. Outputs a plan file.

### `/plan-harness`

Execution harness for plan files. Reads a plan, classifies scope (light/standard/heavy), proposes scoped permissions, sets up a worktree, dispatches subagents to implement, then batches the result into a clean commit stack where each commit is one intention. Persona-based review gates commits on standard/heavy plans. Includes a simplification pass at the end. The user's working tree stays untouched.

## Install

Copy `skills/` into your project or `~/.claude/skills/`.
