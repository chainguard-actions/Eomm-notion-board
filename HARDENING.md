<!-- markdownlint-disable -->

# Hardening Report: Eomm--notion-board/v0.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--notion-board/v0.7.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow steps use mutable tag-based action references instead of pinned full-length SHA commit hashes, making the workflow vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

Failing references in .github/workflows/release.yml:
- uses: actions/checkout@v3
- uses: actions/setup-node@v3
- uses: nearform/optic-release-automation-action@v4

Failing references in .github/workflows/tester.yml:
- uses: actions/checkout@v3

All of these should be pinned to a full 40-character commit SHA (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3).

Locations:

- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:24`
- `.github/workflows/release.yml:27`
- `.github/workflows/tester.yml:10`

### missing-permissions (severity: medium)

The workflow file .github/workflows/tester.yml has no top-level `permissions:` key and its only job (github-action-notion-statusboard) also has no job-level `permissions:` key. Without explicit permissions, the job inherits the default repository token permissions, which may be broader than necessary. A minimal permissions block (e.g. `permissions: {}` or `permissions: contents: read`) should be added.

Locations:

- `.github/workflows/tester.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by replacing mutable tag references with full 40-character commit SHAs (with original tags preserved as comments): actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610, nearform/optic-release-automation-action@v4 → @08642f7889f3bb519fb69ce2c3bf58e4f900c018. Added top-level `permissions: {}` to tester.yml to enforce least-privilege access.

