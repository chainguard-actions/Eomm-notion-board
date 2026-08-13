<!-- markdownlint-disable -->

# Hardening Report: Eomm--notion-board/v0.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--notion-board/v0.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags instead of immutable 40-character SHA commit hashes, making the workflows vulnerable to supply-chain attacks if the referenced tags are moved or overwritten.

In .github/workflows/release.yml:
- `uses: actions/checkout@v3` (tag, not SHA)
- `uses: actions/setup-node@v3` (tag, not SHA)
- `uses: nearform/optic-release-automation-action@v4` (tag, not SHA)

In .github/workflows/tester.yml:
- `uses: actions/checkout@v3` (tag, not SHA)

Locations:

- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:26`
- `.github/workflows/tester.yml:10`

### missing-permissions (severity: medium)

The workflow file .github/workflows/tester.yml has no top-level `permissions:` key and its only job (`github-action-notion-statusboard`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default repository token permissions, which may be broader than necessary (e.g., write access to contents). A minimal `permissions:` block (e.g., `contents: read`) should be added.

Locations:

- `.github/workflows/tester.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all four unpinned action references to full 40-character commit SHAs (actions/checkout@v3, actions/setup-node@v3, nearform/optic-release-automation-action@v4 in release.yml; actions/checkout@v3 in tester.yml). Added top-level `permissions: contents: read` block to tester.yml to enforce least-privilege access.

