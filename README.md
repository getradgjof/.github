# Shared GitHub configuration

This repository provides organization-wide GitHub configuration, including an issue form for backlog feature requests and a label catalog with priorities.

## Issue templates

- Backlog feature request: `.github/ISSUE_TEMPLATE/backlog_feature_request.yml`
  - Fields:
    - Formal documentation URL (e.g., Confluence)
    - Related GitHub Discussion link
    - Problem statement
    - Proposed solution / scope (with in/out of scope)
    - Priority (P0–P3)
    - Size estimate (T-shirt)
    - Acceptance criteria (checkbox-style list)
    - Readiness checklist
    - Security and privacy considerations
    - Dependencies and related work
    - Additional context

Repositories in the org automatically inherit templates from this `.github` repo (on the default branch) under `.github/ISSUE_TEMPLATE`.

Helpful links:
- Confluence: https://confluence.example.com/
- Discussions: https://github.com/<org>/<repo>/discussions

## Org-wide labels

The label catalog lives at `.github/labels.yml` and defines:
- Priority labels: `P0 - Critical`, `P1 - High`, `P2 - Medium`, `P3 - Low`
- Status labels: `status: backlog`, `status: planned`, `status: in progress`, `status: blocked`
- Type label: `type: feature`

### Sync labels to repositories

Use the reusable workflow from consumer repositories to align labels:

```yaml
name: Sync labels from org catalog
on:
  workflow_dispatch:

jobs:
  labels:
    uses: getradgjof/.github/.github/workflows/sync-labels.yml@main
    permissions:
      contents: read
      issues: write
    with:
      # optional overrides
      labels_source_repo: getradgjof/.github
      labels_source_ref: main
      dry_run: false
      skip_delete: false
```

You can also trigger the workflow directly in this repo via the Actions tab to sync labels here. The issue form applies labels `type: feature` and `status: backlog` by default; ensure the catalog sync has run at least once in each repo.

## Notes

- You can customize wording and fields as your process evolves.
- The Priority dropdown in the form is for triage; the auto-apply workflow will add the corresponding `P#` label when the issue is created/updated.
