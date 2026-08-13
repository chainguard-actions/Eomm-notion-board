<!-- markdownlint-disable -->

# Hardening Report: Eomm--notion-board/v0.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--notion-board/v0.6.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tag refs instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved.

.github/workflows/release.yml:
  - uses: actions/checkout@v3 (line 22)
  - uses: actions/setup-node@v3 (line 23)
  - uses: nearform/optic-release-automation-action@v4 (line 27)

.github/workflows/tester.yml:
  - uses: actions/checkout@v3 (line 9)

All should be pinned to their full SHA digest, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3

Locations:

- `.github/workflows/release.yml:22`
- `.github/workflows/tester.yml:9`

### missing-permissions (severity: medium)

The workflow file .github/workflows/tester.yml has no top-level `permissions:` key and its only job (github-action-notion-statusboard) also has no job-level `permissions:` key. Without explicit permissions, the job inherits the default repository token permissions, which may be overly broad. A minimal permissions block (e.g. `permissions: {}` or `permissions: contents: read`) should be added.

Locations:

- `.github/workflows/tester.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving their full commit SHAs: actions/checkout@v3 → f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, nearform/optic-release-automation-action@v4 → 08642f7889f3bb519fb69ce2c3bf58e4f900c018. Original tags preserved as inline comments. Added `permissions: {}` at the top level of tester.yml to enforce least-privilege (the workflow uses only secrets and a local action, requiring no repository token permissions).

