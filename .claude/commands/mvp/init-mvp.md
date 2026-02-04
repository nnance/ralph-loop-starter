---
description: Create the MVP phase supporting documents for Ralph Wiggum autonomous development loop based on the PRD
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Create Supporting Documents for Ralph Wiggum Autonomous Development

Your goal is to create the necessary supporting documents to enable autonomous development using the Ralph Wiggum loop. This includes generating a task list in `PLAN.md`, updating `PROMPT.md` with project-specific instructions, and modifying `.claude/settings.json` to ensure the agent has the required permissions.

## Phase 1: Read the PRD

Read the entire `PRD.md` file to understand the project requirements, features, and technical specifications. Take notes on:

- Core features and functionalities
- Tech stack and frameworks
- Any special instructions or considerations

## Phase 2: Generate Plan File

Based on the PRD, create a detailed task list in `PLAN.md`. Use the following template for the file. Each task should be atomic, verifiable, and ordered logically. Use the following structure for each task:

````markdown
# Project Plan

## Overview

Brief description of what you're building.

**Reference:** `PRD.md`

---

## Task List

```json
[
  {
    "category": "setup",
    "description": "[First setup task]",
    "steps": ["[Step 1]", "[Step 2]", "[Step 3]"],
    "passes": false
  },
  {
    "category": "feature",
    "description": "[Feature task]",
    "steps": ["[Step 1]", "[Step 2]"],
    "passes": false
  }
]
```

---

## Agent Instructions

1. Read `activity.md` first to understand current state
2. Find next task with `"passes": false`
3. Complete all steps for that task
4. Verify in browser using agent-browser
5. Update task to `"passes": true`
6. Log completion in `activity.md`
7. Repeat until all tasks pass

**Important:** Only modify the `passes` field. Do not remove or rewrite tasks.

---

## Completion Criteria

All tasks marked with `"passes": true`
````

### Task Generation Guidelines

Generate tasks based on the features and requirements gathered. Tasks should be:

- **Atomic**: Each task should be completable in one iteration
- **Verifiable**: Each task should have clear success criteria
- **Ordered**: Tasks should be in logical dependency order
- **Categorized**: Use categories like `setup`, `feature`, `integration`, `styling`, `testing`

**Typical task categories:**

1. **setup**: Project initialization, dependencies, configuration
2. **feature**: Core feature implementations
3. **integration**: Third-party service integrations
4. **styling**: UI/UX implementation
5. **testing**: Test coverage and verification

## Phase 3: Update PROMPT.md

After creating the PRD, update the `PROMPT.md` file to reflect the project specifics.

Read the current `PROMPT.md` file and update the following sections:

1. **Start command**: Replace the placeholder with the actual command to start the dev server (based on tech stack chosen)
2. **Build/lint commands**: Add any relevant build or lint commands
3. **Project-specific instructions**: Add any special considerations from the PRD

Use the Edit tool to update these sections while preserving the rest of the template.

## Phase 4: Create Supporting Files

After creating the PRD and updating PROMPT.md, create any additional supporting files needed for Ralph Wiggum autonomous development.

1. **Create activity.md** if it doesn't exist:

```markdown
# [Project Name] - Activity Log

## Current Status

**Last Updated:** [Current Date]
**Tasks Completed:** 0
**Current Task:** None started

---

## Session Log

<!-- Agent will append dated entries here -->
```

2. Confirm to the user that all files are ready for Ralph Wiggum autonomous development.

## Phase 5: Final Verification Prompt

After completing all phases, present the user with a verification checklist:

```
Your Plan is ready! Before running ralph.sh, please verify:

**PLAN.md**
- [ ] All features captured in task list
- [ ] Tasks are atomic and verifiable
- [ ] Tasks are in correct dependency order
- [ ] Success criteria is clear

**PROMPT.md:**
- [ ] Start command is correct for your tech stack
- [ ] Build/lint commands are accurate
- [ ] Verification steps are clear
- [ ] Project-specific instructions are included

Once verified, run: ./ralph.sh 20

Monitor progress in activity.md and screenshots/
```

Explicitly tell the user to verify these files before running the loop. This verification step is critical for a successful Ralph Wiggum run.
