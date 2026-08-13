<!-- markdownlint-disable -->

# Hardening Report: Eomm--notion-board/v0.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--notion-board/v0.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Failing references: release.yml — `actions/checkout@v3`, `actions/setup-node@v3`, `nearform/optic-release-automation-action@v4`; tester.yml — `actions/checkout@v3`.

Locations:

- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:26`
- `.github/workflows/tester.yml:10`

### missing-permissions (severity: medium)

The workflow file tester.yml has no top-level `permissions:` key and its only job (`github-action-notion-statusboard`) also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository token permissions, which may be broader than necessary (e.g. write access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes should be added.

Locations:

- `.github/workflows/tester.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full 40-character commit SHAs (with original tags preserved as comments): actions/checkout@v3 → SHA a37ce91, actions/setup-node@v3 → SHA 3235b87, nearform/optic-release-automation-action@v4 → SHA 08642f7. Added top-level `permissions: {}` to tester.yml to restrict the workflow token to no permissions, as the workflow only uses secrets and a local action and requires no GitHub token access.

