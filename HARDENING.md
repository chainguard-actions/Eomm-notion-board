<!-- markdownlint-disable -->

# Hardening Report: Eomm--notion-board/v0.4.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--notion-board/v0.4.2** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or overwritten. Failing references: release.yml uses `actions/checkout@v3`, `actions/setup-node@v3`, and `nearform/optic-release-automation-action@v4`; tester.yml uses `actions/checkout@v3`. All should be pinned to a full SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`).

Locations:

- `.github/workflows/release.yml:24`
- `.github/workflows/release.yml:25`
- `.github/workflows/release.yml:28`
- `.github/workflows/tester.yml:10`

### missing-permissions (severity: medium)

The workflow file tester.yml has no top-level `permissions:` key and the single job (`github-action-notion-statusboard`) also has no `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary (e.g. write access to contents). A minimal `permissions: read-all` or specific scopes should be declared.

Locations:

- `.github/workflows/tester.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full 40-character commit SHAs (with tag comments for readability): actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, nearform/optic-release-automation-action@v4 → 08642f7889f3bb519fb69ce2c3bf58e4f900c018. Added `permissions: read-all` at the top level of tester.yml to address the missing-permissions finding.

### Iteration 2

**Fixes applied:** broad-permissions

**Notes:**

Replaced `permissions: read-all` with specific minimal permissions `contents: read` in `.github/workflows/tester.yml`. The workflow only performs a repository checkout and runs a local action, so `contents: read` is the only permission required.

