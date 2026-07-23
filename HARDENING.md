<!-- markdownlint-disable -->

# Hardening Report: NuGet--setup-nuget/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **NuGet--setup-nuget/v1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag/version refs instead of pinned 40-character commit SHAs, making the action vulnerable to supply-chain attacks if those tags are moved or compromised.

`.github/workflows/codeql.yml`:
- `uses: actions/checkout@v2` (line 19)
- `uses: github/codeql-action/init@v1` (line 27)
- `uses: github/codeql-action/autobuild@v1` (line 33)
- `uses: github/codeql-action/analyze@v1` (line 44)

`.github/workflows/test.yml`:
- `uses: actions/checkout@v1` (line 9)

Locations:

- `.github/workflows/codeql.yml:19`
- `.github/workflows/codeql.yml:27`
- `.github/workflows/codeql.yml:33`
- `.github/workflows/codeql.yml:44`
- `.github/workflows/test.yml:9`

### missing-permissions (severity: medium)

Neither `.github/workflows/codeql.yml` nor `.github/workflows/test.yml` has a top-level `permissions:` key, and no job in either file defines its own `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/codeql.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references in codeql.yml and test.yml by resolving each tag to its full 40-character commit SHA using lookup_action_sha. Added top-level permissions blocks to both workflow files: codeql.yml gets 'contents: read' and 'security-events: write' (required for CodeQL to upload SARIF results), while test.yml gets 'contents: read' (minimal permission for checkout). Original tags are preserved as inline comments for readability.

