<!-- markdownlint-disable -->

# Hardening Report: NuGet--setup-nuget/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **NuGet--setup-nuget/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable tags rather than full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved.

codeql.yml:
- Line 24: `uses: actions/checkout@v6`
- Line 32: `uses: github/codeql-action/init@v4`
- Line 38: `uses: github/codeql-action/autobuild@v4`
- Line 48: `uses: github/codeql-action/analyze@v4`

test.yml:
- Line 14: `uses: actions/checkout@v6`

All of these should be pinned to a full SHA digest (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v6`).

Locations:

- `.github/workflows/codeql.yml:24`
- `.github/workflows/codeql.yml:32`
- `.github/workflows/codeql.yml:38`
- `.github/workflows/codeql.yml:48`
- `.github/workflows/test.yml:14`

### missing-permissions (severity: medium)

Neither `.github/workflows/codeql.yml` nor `.github/workflows/test.yml` declares a `permissions:` key at the top level or at the job level. Without explicit permissions, workflows inherit the repository's default token permissions (which may be `write-all`), granting unnecessarily broad access. Each workflow should declare minimal required permissions (e.g. `permissions: read-all` or specific scopes).

Locations:

- `.github/workflows/codeql.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned `uses:` references by pinning them to full 40-character commit SHAs (with tag comments for readability). Added minimal `permissions:` blocks to both workflow files: codeql.yml gets `contents: read` + `security-events: write` (required for CodeQL to upload SARIF results), and test.yml gets `contents: read` only.

