# Ralph Wiggum: Autonomous Development Workflow Project Starter

> **Credit:** This guide is inspired by [JeredBlu's Ralph Wiggum Guide](https://github.com/JeredBlu/guides/blob/main/Ralph_Wiggum_Guide.md) and [Cole Medin's work](https://github.com/coleam00/ralph-loop-quickstart). I've adapted and extended it to fit my specific workflow where I build autonomous development loops by:

**MVP Phase** Project creation and the initial Ralph Wiggum Loop

- I Start all projects by creating the intitial MVP phase to ensure the project foundation is solid by generating a comprehensive PRD with the `/mvp:create-prd` command and the `/mvp:init-mvp` command.
- Then running the Ralph Wiggum autonomous development loop with the `ralph.sh` bash script.

**Feature Development Phases** and Ralph Wiggum Looping

- After the MVP phase, I create additional feature development phases as needed using the `/init-loop` command to generate new task plans based on a new set of features defined in the PRD.
- Then I run the Ralph Wiggum loop again with `ralph.sh` to implement those features autonomously.

---

## What is Ralph Wiggum?

Ralph Wiggum is a method for running Claude Code in a continuous autonomous loop. Each iteration runs in a **fresh context window**, allowing the agent to work through a series of tasks until completion without context bloat.

### When to Use Ralph Wiggum

**Ideal for:**

- Starting projects from scratch
- Building proof of concepts (POCs) with a clearly defined scope
- Greenfield development where requirements are well-understood
- Projects where you can define "done" for each phase clearly

**Not ideal for:**

- Complex existing codebases
- Vibe coding or exploratory work without clear goals
- Projects with multiple contributors requiring coordination
- Situations requiring frequent human judgment calls

The key insight: **Ralph Wiggum works best when you have a clear plan.** If you don't know exactly what you're building, stop and figure that out first. The `/mvp:create-prd` command helps you do exactly that.

---

## Prerequisites

### 1. Claude Code

You need Claude Code installed and configured. See the [official documentation](https://docs.anthropic.com/claude-code) for setup.

### 2. Vercel Agent Browser CLI

Install the agent-browser CLI for headless browser automation:

```bash
npm install -g agent-browser
agent-browser install  # Downloads Chromium
```

On Linux, include system dependencies:

```bash
agent-browser install --with-deps
```

This tool allows Claude to verify its work visually by taking screenshots and interacting with your running application.

### 3. Project Setup Files

This repository includes everything you need:

- `.claude/settings.json` - Sandbox and permissions configuration
- `.claude/commands/mvp/create-prd.md` - The `/mvp:create-prd` command for creating the MVP PRD
- `.claude/commands/mvp/init-mvp.md` - The `/mvp:init-mvp` command for creating the MVP plan
- `.claude/commands/init-loop.md` - The `/init-loop` command for feature phases
- `.claude/skills/agent-browser-skill/SKILL.md` - Agent browser instructions
- `PROMPT.md` - Template for the Ralph loop
- `ralph.sh` - The bash loop script
- `activity.md` - Activity logging template
- `screenshots/` - Directory for visual verification

---

## Getting Started

This project is setup as a template so you can clone it and start building your own Ralph Wiggum projects quickly. Create a new project by clicking on the "Use this template" button on GitHub to create a new repository based on this one.

Then follow the steps below to create your MVP phase and start the Ralph Wiggum loop.

---

## The MVP Process

### Step 1: Create Your PRD with `/mvp:create-prd`

If you have an existing PRD, you can skip this step. Otherwise, follow these steps to create a solid PRD. Your PRD must be very clear about what you're building for Ralph Wiggum to work effectively and it is recommended that it follows the structure below.

Run the `/mvp:create-prd` command in Claude Code:

This interactive command will:

1. **Ask discovery questions** one at a time:
   - What problem are you solving?
   - Who is your target audience?
   - What are the 3-5 core features?
   - What tech stack do you want?
   - What architecture approach?
   - UI/UX preferences?
   - Authentication requirements?
   - Third-party integrations?
   - Success criteria?

2. **Research options** if you're unsure about tech stack or architecture

3. **Generate `PRD.md`** with:
   - Complete project requirements
   - Tech stack decisions
   - JSON task list with atomic, verifiable tasks

### Step 2: Initialize the MVP Phase with `/mvp:init-mvp`

1. **Create `PLAN.md`** with a detailed task list based on the PRD

2. **Update `PROMPT.md`** with your specific:
   - Start commands for your tech stack
   - Build/lint commands
   - Project-specific instructions

3. **Update `.claude/settings.json`** with permissions for:
   - CLI tools required by your tech stack
   - Any third-party CLIs needed
   - Commands specific to your project

4. **Create/verify `activity.md`** for logging progress

### Step 3: Verify Your Setup

After `/mvp:init-mvp` completes, **verify these files before running the loop**:

**Check `PLAN.md`:**

- Are all features captured in the task list?
- Are tasks atomic (completable in one iteration)?
- Are tasks in the correct dependency order?
- Is the success criteria clear?

**Check `PROMPT.md`:**

- Is the start command correct for your tech stack?
- Are build/lint commands accurate?

**Check `.claude/settings.json`:**

- Are all necessary CLI tools permitted?
- Are sensitive files properly denied?
- Does the agent have what it needs to work autonomously?

This verification step is **critical**. The quality of your Ralph Wiggum run depends entirely on the quality of your PRD and configuration.

### Step 4: Run the Ralph Loop

Once verified, start the autonomous loop:

```bash
./ralph.sh 20
```

The number is your maximum iterations. Start with 10-20 for smaller projects.

The script will:

1. Read `PROMPT.md` and feed it to Claude
2. Claude works on one task from `PLAN.md`
3. Verifies with agent-browser
4. Updates task status and logs to `activity.md`
5. Commits the change
6. Repeats with a fresh context window

The loop exits when:

- All tasks have `"passes": true` (outputs `<promise>COMPLETE</promise>`)
- Max iterations reached

### Step 5: Monitor Progress

While Ralph runs, you can monitor:

- **`activity.md`** - Detailed log of what was accomplished each iteration
- **`screenshots/`** - Visual verification of each completed task
- **Git commits** - One commit per task with clear messages
- **Terminal output** - Real-time progress

## Feature Development Phases

After completing the MVP phase, you can add new features by:

1. **Create a new `PRD.md`** with new features and requirements
2. **Running `/init-loop`** to create a new `PLAN.md` with tasks for the new features
3. **Running the Ralph loop** again to implement and verify new features
4. **Monitoring progress** as before watching `activity.md` and `screenshots/`

Ralph Wiggum can be run multiple times for different phases of development, each time starting with a fresh plan and task list. Carefully managing scope and requirements for each phase is key to success.

---

## File Reference

### `PLAN.md` (Generated)

Your Product Requirements Document with a JSON task list:

```json
[
  {
    "category": "setup",
    "description": "Initialize Next.js project with TypeScript",
    "steps": [
      "Run create-next-app with TypeScript template",
      "Install additional dependencies",
      "Verify dev server starts"
    ],
    "passes": false
  }
]
```

Tasks should be:

- **Atomic**: One logical unit of work
- **Verifiable**: Clear success criteria
- **Ordered**: Respect dependencies
- **Categorized**: setup, feature, integration, styling, testing

### `PROMPT.md`

Instructions for each iteration. References `@PLAN.md`, `PRD.md` and `@activity.md`. Updated by `/mvp:init-mvp` with your specific start commands.

### `.claude/settings.json`

Permissions and sandbox configuration. Updated by `/mvp:init-mvp` based on your tech stack to ensure the agent can run all necessary commands.

Example permissions that might be added based on your PRD:

- `Bash(prisma:*)` for Prisma CLI
- `Bash(supabase:*)` for Supabase CLI
- `Bash(firebase:*)` for Firebase CLI
- `Bash(vercel:*)` for Vercel CLI
- `Bash(docker compose:*)` for Docker workflows

### `ralph.sh`

The bash loop script. Key features:

- Fresh context window per iteration
- Completion detection via `<promise>COMPLETE</promise>`
- File existence validation
- Color-coded output
- Graceful handling of max iterations

### `activity.md`

Activity log where the agent records:

- Date and time
- Task worked on
- Changes made
- Commands run
- Screenshot filename
- Issues and resolutions

---

## Best Practices

### 1. Plan Thoroughly

The `/mvp:create-prd` command exists because **planning is everything**. Don't skip the discovery questions. Don't rush through them. A well-defined PRD is the difference between a successful Ralph run and wasted API credits.

### 2. Keep Scope Tight

Ralph Wiggum is for proof of concepts, not the final application. Define the minimum viable version of your idea. You can always iterate later.

### 3. Verify Before Running

Always review `PLAN.md`, `PROMPT.md`, and `.claude/settings.json` before starting the loop. Catching issues here saves iterations.

### 4. Start with Fewer Iterations

Use 10-20 iterations initially. You can always run more if needed. This prevents runaway costs if something goes wrong.

### 5. Monitor the First Few Iterations

Watch the first 2-3 iterations to ensure things are working correctly. Check that:

- The dev server starts properly
- Agent-browser can access localhost
- Tasks are being marked as passing correctly
- Activity log is being updated

### 6. Use Sandboxing

The default `.claude/settings.json` enables sandboxing. This provides isolation for long-running autonomous tasks. Don't disable it unless you have a specific reason.

---

## Troubleshooting

### Agent gets stuck in a loop

- Check if the task is too ambiguous
- Review `activity.md` to see what it's attempting
- Consider breaking the task into smaller steps

### Agent can't access localhost

- Verify the dev server is running
- Check that the port in `PROMPT.md` matches your actual port
- Ensure agent-browser is installed correctly

### Permissions errors

- Review `.claude/settings.json`
- Add missing CLI tools to the allow list
- Check that the command pattern matches (e.g., `Bash(npm run:*)`)

### Context issues / hallucinations

- This shouldn't happen with the bash loop (fresh context each iteration)
- If using the plugin (not recommended), switch to `ralph.sh`

### Max iterations reached without completion

- Review remaining tasks in `prd.md`
- Check if tasks are too large or ambiguous
- Run again with `./ralph.sh 30` or more iterations

---

## Links

- [Original Ralph Wiggum Guide by JeredBlu](https://github.com/JeredBlu/guides/blob/main/Ralph_Wiggum_Guide.md)
- [Vercel Agent Browser CLI](https://github.com/vercel-labs/agent-browser)
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [Anthropic's Long-Running Agents Blog Post](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
