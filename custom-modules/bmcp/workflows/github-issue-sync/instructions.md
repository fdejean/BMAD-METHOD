# GitHub Issue Sync

## Objective
Update GitHub issues for a story and its tasks after development, based on the story status and task completion.

## Preconditions
- `github_repo` is set (format: `owner/name`).
- `story_key` is provided.
- `issue_map_file` exists and contains issue URLs for the story and tasks.

If GitHub MCP tools are unavailable or misconfigured, prompt the user to fix MCP setup and retry. Then HALT.

## Steps

### Step 1: Load Story Status
1. If `story_file` is provided, load it and read the `Status` section.
2. If `story_file` is empty, ask the user for the story file path and load it.
3. Capture:
   - Story status (e.g., `review`, `done`)
   - Completed tasks list (checked [x])

### Step 2: Resolve Issue URLs
1. Load `issue_map_file`.
2. Find the section for `story_key` and extract:
   - Story issue URL
   - Task issue URLs
3. If no issue URLs are found, instruct the user to run `mcp-issue-orchestrator` and HALT.

### Step 3: Update GitHub Issues
Use GitHub MCP to update issues:
- If story status is `review`, add label: `<issue_label_prefix>-review` to the story issue.
- If story status is `done`, close the story issue and add label: `<issue_label_prefix>-done`.

For each completed task:
- Close the task issue and add label: `<issue_label_prefix>-done`.

If the user prefers, ask whether to leave incomplete task issues open.

### Step 4: Report
Summarize updates:
- Story issue updated (status/labels)
- Tasks closed
- Any tasks left open
