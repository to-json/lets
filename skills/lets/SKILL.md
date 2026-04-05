---
name: lets
description: >
  Deliberative planning skill for significant proposals. Takes a plan idea — from a vague intention
  to a detailed proposal — interviews the user to understand context and intent, reviews repo state
  and documentation, drafts a structured plan, then runs bimodal review (general + adversarial) and
  synthesizes. Use this skill when the user says "/lets", proposes a new feature or architectural
  change, wants to think through a significant piece of work before committing to it, or when the
  scope clearly exceeds what /plan's 3-minute tactical format can cover. Also trigger when the user
  says things like "I want to...", "we should...", "what if we...", "I'm thinking about..." followed
  by something that sounds like a project, not a quick fix.
---

# /lets — Deliberative Planning

Take a proposal from intention to structured plan through interview, drafting, bimodal review, and synthesis. The output is a plan file in the project's plan directory.

The whole point: plans that survive contact with reality because they've already been stress-tested before a line of code is written.

## Phase 1: Orient

Read enough to be a competent interviewer — not an exhaustive survey.

1. Look for a plan sequence or index file (e.g., `plans/PLAN-SEQUENCE.md`, `plans/index.md`, or similar) to understand active work and dependencies.
2. Scan the plan directory for filenames. If any look related to the proposal, read their context sections.
3. If the proposal names specific subsystems, read relevant source files or docs (especially project-level `CLAUDE.md`, cleanup tracking files, architecture docs). Stay focused — read what's relevant, not everything.
4. Check for prior art: has this been attempted, deferred, or explicitly rejected? Look in sequence files, changelogs, and completed/archived plans.

Time budget: under 2 minutes. You're building context for the interview, not writing the plan yet.

## Phase 2: Interview

The interview has one job: extract enough understanding to draft a plan the user would recognize as capturing their intent.

**Calibrate depth to perceived importance.** A proposal that touches auth, data integrity, or architecture across multiple subsystems deserves 4-6 questions. A self-contained feature addition might need 2-3. Use judgment — if the user arrived with a detailed proposal, don't re-ask what they already told you. Extract what's missing.

**Core questions** (select and adapt, don't recite mechanically):

- What's the problem this solves? (If not obvious from the proposal)
- What does success look like — what's different when this is done?
- What's the scope boundary — what's explicitly NOT part of this?
- Are there ordering constraints with other active work? (Reference what you found in the plan directory)
- Who/what depends on this? Who/what does this depend on?
- Is there a forcing function — deadline, blocking issue, user pain?
- What's the riskiest part? What are you least sure about?

**Interview style**: Direct questions, one or two at a time. Don't dump all questions at once. React to answers — follow up on surprising or ambiguous responses. If the user says something that contradicts what you found in the repo, surface it immediately.

## Phase 3: Draft

Write a first draft of the plan. If the project has established plan conventions (consistent format across existing plans), match them. Otherwise use this structure:

```markdown
# {Title} — {Subtitle}

Date: {today}
Depends on: {plan names, or "None"}

---

## Context

{Problem statement. Why this matters. What's broken or missing.
Include substrate/architectural notes where relevant — the "why behind the why."
2-4 paragraphs.}

---

## Stream N: {Stream title}

**Problem**: {What specifically is wrong or missing}

**File(s)**: `{paths}`

### N.1 {Step title}

{Description of the change. Enough detail that an implementer knows
what to build without ambiguity, but not so much that it's pseudocode.}

### N.2 ...

---

{Repeat streams as needed. Most plans have 2-5 streams.}

## Sequence integration

{Where this plan fits relative to other active work. What it blocks,
what blocks it. If it affects other plans' scheduling, note that.}

## Risks

- {Risk and mitigation, max 5}
```

**Conventions to follow:**
- Streams are parallelizable work units within the plan
- Subsections (N.1, N.2) are sequential within a stream
- Reference specific file paths where known
- Keep the Context section honest about what you don't know yet
- If a decision could go either way, flag it explicitly rather than picking silently

Present the draft to the user. Don't save it yet.

## Phase 4: Bimodal Review

Run two reviews of the draft, then synthesize. These can run in parallel as subagents if available, or sequentially if not.

### General Review

Adopt the lens of a senior engineer doing a design review. Evaluate:

- **Completeness**: Does the plan cover the full scope the user described? Are there gaps between intent and specification?
- **Feasibility**: Can this actually be built as described? Are the file paths real? Do the proposed changes align with how the code actually works?
- **Dependency correctness**: Are the stated dependencies accurate? Are there unstated dependencies — other plans, external systems, data migrations?
- **Sequence integration**: Does the proposed placement relative to other work make sense? Does it create circular dependencies? Does it block or get blocked by anything not mentioned?
- **Clarity**: Could an implementer pick this up and start working without asking clarifying questions?

### Adversarial Review

Adopt the lens of someone whose job is to find why this plan will fail. Attack:

- **Hidden assumptions**: What is the plan taking for granted that might not be true? What would break if those assumptions are wrong?
- **Second-order effects**: What happens to other active work if this ships? What happens to existing users/data? What breaks during the transition?
- **Scope creep vectors**: Where will this plan's scope want to expand? What adjacent work will feel "natural" to include but shouldn't be?
- **The hard part**: Is the plan honest about where the real difficulty is, or does it hand-wave the gnarliest bit? Is there a "then a miracle occurs" step?
- **Sequencing risk**: Could executing this in the proposed order leave the system in a broken intermediate state? What if a stream is half-done and gets interrupted?
- **What if we're wrong**: What's the cost of building this and discovering the approach was wrong? Is the plan structured so we learn fast (riskiest-first) or so we learn too late?

### Metacog integration (if available)

If metacog MCP tools are available:
- General reviewer: `mcp__metacog__become` — name: "a principal engineer who has mass-reverted three 'great plans' this year", lens: "does this plan actually need to exist and if so does it solve the right problem", environment: contextual to the proposal
- Adversarial reviewer: `mcp__metacog__become` — name: "Nassim Taleb reviewing an engineering plan", lens: "fragility detection — what looks robust but breaks under stress, what looks risky but is actually antifragile", environment: contextual to the proposal
- Both should `mcp__metacog__feel` before articulating — let the non-obvious concerns surface

If metacog is not available, proceed without it. The skill works either way.

## Phase 5: Synthesis

Read both reviews. Sit with them. Then produce a synthesis that:

1. **Identifies the real tensions** — where the general and adversarial reviews disagree, or where one surfaced something the other missed
2. **Proposes specific revisions** to the draft plan, with reasoning
3. **Flags open questions** that the reviews raised but couldn't resolve — these go back to the user

Present the synthesis to the user alongside the two reviews. Then ask:

> "Here's where I think the draft and the synthesis diverge. {Describe the key deltas.} What's your read — should we pull the plan toward the synthesis, hold the original direction, or is there a third option I'm not seeing?"

This is a conversation, not a deliverable. The user might agree with the adversarial review on one point and dismiss it on another. Adjust.

## Phase 6: Finalize and Save

After the user confirms direction:

1. Revise the draft incorporating agreed-upon changes
2. Determine save location:
   - If the project has a plan directory (e.g., `plans/`) → save there
   - Otherwise ask the user
3. If a plan sequence/index file exists, update it:
   - Add the new plan to the appropriate tier or section
   - Update dependency graphs if the plan creates new edges
   - Add to a "deferred" or "parked" section if the user indicates it's not for immediate execution
4. Save and confirm:

> "Plan written to `{path}`. Run `/plan-harness` to implement."

## Rules

- Do NOT write code or start implementing. This is planning only.
- Do NOT skip the interview. Even if the proposal seems clear, validate assumptions.
- Do NOT skip the bimodal review. The adversarial pass is the whole point.
- Save the plan only after the user has seen the synthesis and confirmed direction.
- If the proposal is small enough for `/plan` (single file, <30 min implementation), say so and suggest `/plan` instead. Don't over-engineer small changes.
