<!-- markdownlint-disable -->

# Hardening Report: Eomm--notion-board/v0.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--notion-board/v0.4.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files use mutable tag-based action references instead of pinned full SHA commits, making them vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

In `.github/workflows/release.yml`:
- `uses: actions/checkout@v3` (line 22)
- `uses: actions/setup-node@v3` (line 23)
- `uses: nearform/optic-release-automation-action@v4` (line 27)

In `.github/workflows/tester.yml`:
- `uses: actions/checkout@v3` (line 10)

All should be pinned to their full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:27`
- `.github/workflows/tester.yml:10`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/tester.yml` has no top-level `permissions:` block and its only job (`github-action-notion-statusboard`) also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository permissions (which may be `write-all` for private repos or `read-all` for public repos), granting broader access than necessary. A minimal `permissions:` block (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/tester.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all four action references to their full commit SHAs (actions/checkout@v3, actions/setup-node@v3, nearform/optic-release-automation-action@v4 in release.yml; actions/checkout@v3 in tester.yml). Added a top-level `permissions: contents: read` block to tester.yml to enforce least-privilege access.

