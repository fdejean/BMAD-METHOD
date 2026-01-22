# Story Implementation Plan (SM)

## Objective
Create a detailed, stable implementation plan for a single story so that task issues can be created and offloaded with minimal ambiguity.

## Preconditions
- A story file exists (from `create-story`) in `{implementation_artifacts}`.
- You know the story file path or can identify it by story key.

If `story_file` is empty, ask the user for the exact path to the story file.

## Output
Write the plan to `{default_output_file}` and update the story file with:
- A short "Implementation Plan" section
- Link/path to the plan file
- The list of task titles (1 line each)
- Date updated

## Steps

### Step 1: Load the Story
1. Load the full story file from `story_file`.
2. Extract:
   - Story key/title
   - Description
   - Acceptance criteria
   - Constraints/notes
3. If `story_key` is empty, derive it from the filename or ask the user.

### Step 2: Create a Story Snapshot Section
In the plan file, include a "Story Snapshot" section with:
- Story key and title
- The full acceptance criteria (verbatim)
- Any constraints or dependencies

This snapshot is used later to detect drift. If the story changes, the plan must be regenerated.

### Step 3: Break Down into Tasks
Create 5-12 tasks (more for large stories):
- Each task is independently testable and bounded
- Prefer sequence with clear dependencies
- Include required tests for each task

For each task, include:
- Title
- Scope and deliverables
- Acceptance criteria
- Test expectations
- Dependencies (if any)

### Step 4: Update the Story for Consistency
Append or update an "Implementation Plan" section in the story file:
- Plan file path
- Task list (titles only)
- Updated date

If the story already has an implementation plan section, update it to match the new plan.

### Step 5: Final Checks
Confirm:
- Every acceptance criterion maps to at least one task
- Task order is logical and minimal
- Plan file and story file are consistent
