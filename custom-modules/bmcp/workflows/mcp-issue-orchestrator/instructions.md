# MCP Issue Orchestrator

## Objective
Publish epics and user stories to GitHub as issues, use SM-created implementation plans to create task issues, and offload tasks to a cloud AI agent via Jules MCP or Codex MCP.

## Preconditions
- `github_repo` is set (format: `owner/name`).
- Epics and stories exist in `{planning_artifacts}` (default: `epics.md`).
- Each target story has an implementation plan in `{implementation_artifacts}/story-plans/`.

If `github_repo` is empty, ask the user for it and update the module config output.
If MCP tools are not configured or unavailable, prompt the user to fix MCP setup and retry. Do not proceed with issue creation or offloading.

## Output
Write a summary to `{default_output_file}` including:
- Epic issues created
- Story issues created
- Task issues created
- Offload status (provider, task ID, issue URL)
- Any errors or manual fallbacks

Use a consistent section format so follow-on workflows can parse it:

```
Story Key: <story_key>
Story Issue: <issue_url>
Tasks:
- <task_title> | <issue_url>
- ...
```

## Execution Steps

### Step 1: Load and Parse Epics/Stories
1. Load epics and stories from `{epics}` input patterns.
2. Build a normalized list:
   - `Epic`: title, description, key (if present)
   - `Story`: title, acceptance criteria, key, parent epic
3. If `story_filter` is set, limit the list to matching epic/story keys or titles.
4. Load matching implementation plans from `{story_plans}`. If a story has no plan:
   - Stop and instruct the user to run the SM planning workflow (`story-implementation-plan`).

### Step 2: Create Epic and Story Issues (GitHub MCP)
Check GitHub MCP availability:
- If GitHub MCP tools are unavailable or misconfigured, prompt the user to fix MCP setup and retry. Then HALT.

Use GitHub MCP tools to create issues.

For each epic:
- Title: `Epic: <Epic Title>`
- Body: epic summary, source reference, scope boundaries, and a **task list** of story issues (parent/child tracking)
- Labels: `<issue_label_prefix>-epic`, `epic`

For each story under the epic:
- Title: `Story: <Story Title>`
- Body: story description + acceptance criteria + dependencies + constraints + `Epic: #<epic_issue_number>` + **task list** of task issues
- Labels: `<issue_label_prefix>-story`, `story`

If `github_project_number` is provided, add each created issue to that project via GitHub MCP.

Record all issue URLs and numbers in the output file as you go.

Parent/Child Tracking Requirements:
- Use GitHub MCP `sub_issue_write` to attach each story issue to its epic.
- Use GitHub MCP `sub_issue_write` to attach each task issue to its story.
- Also include GitHub task list syntax in the epic and story bodies for visibility:
  - `- [ ] #<child_issue_number> <child_title>`

### Step 3: Create Task Issues from Implementation Plans
For each story, use the corresponding implementation plan to create **non-trivial, implementation-ready** task issues:
- Create 5-12 tasks for normal stories; more for larger scope.
- Each task must be independently deliverable, testable, and non-trivial.
- Prefer tasks that map to concrete code changes, not generic "do X" steps.
- Titles use prefix: `Task: <Action>`
- Bodies include:
  - Summary and intent
  - Technical steps (key files, components, APIs)
  - Acceptance criteria (clear and testable)
  - Test expectations (unit/integration/e2e)
  - Dependencies/ordering constraints
  - Related story issue: `Story: #<story_issue_number>`

If a plan includes a "Story Snapshot" section, verify the story still matches.
If the story diverged, stop and instruct SM to regenerate the plan.

Create each task as a GitHub issue via MCP with labels:
- `<issue_label_prefix>-task`, `task`

### Step 4: Offload Tasks via Jules MCP or Codex MCP
Select provider:
- If `offload_provider=auto`, prefer Jules MCP if available, else Codex MCP.

Check offload MCP availability:
- If the selected provider is unavailable or misconfigured, prompt the user to fix MCP setup and retry.
- If no MCP tools are available, ask whether to proceed with issue creation only or halt entirely.

For each task issue:
1. Prepare a concise execution brief (max 10 lines):
   - Issue URL + title
   - Repo and branch policy
   - Expected deliverable and tests
   - Constraints or dependencies
2. Use the chosen MCP tool to submit the task.
3. Capture the returned task/job ID.

If no offload MCP tool is available, mark as `manual` in the output.

### Step 5: Final Summary
Update `{default_output_file}` with a table:
- Epic/Story/Task
- GitHub issue URL
- Offload provider and task ID (if any)

End with a short checklist of completed actions and remaining manual steps.
