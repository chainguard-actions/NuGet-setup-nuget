<!-- markdownlint-disable -->

# Hardening Report: NuGet--setup-nuget/v2.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **NuGet--setup-nuget/v2.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow steps use mutable tag-based refs instead of full 40-character SHA commit hashes, making the workflow vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

.github/workflows/codeql.yml:
- `uses: actions/checkout@v2` (line 20)
- `uses: github/codeql-action/init@v1` (line 32)
- `uses: github/codeql-action/autobuild@v1` (line 38)
- `uses: github/codeql-action/analyze@v1` (line 50)

.github/workflows/test.yml:
- `uses: actions/checkout@v1` (line 8)

Locations:

- `.github/workflows/codeql.yml:20`
- `.github/workflows/codeql.yml:32`
- `.github/workflows/codeql.yml:38`
- `.github/workflows/codeql.yml:50`
- `.github/workflows/test.yml:8`

### missing-permissions (severity: medium)

Neither .github/workflows/codeql.yml nor .github/workflows/test.yml defines a top-level `permissions:` key, and no individual job within either file defines its own `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/codeql.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

.github/workflows/codeql.yml:
- Pinned actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e # v2
- Pinned github/codeql-action/init@v1 → @231aa2c8a89117b126725a0e11897209b7118144 # v1
- Pinned github/codeql-action/autobuild@v1 → @231aa2c8a89117b126725a0e11897209b7118144 # v1
- Pinned github/codeql-action/analyze@v1 → @231aa2c8a89117b126725a0e11897209b7118144 # v1
- Added top-level permissions: { contents: read, security-events: write } (security-events write is required for CodeQL to upload SARIF results)

.github/workflows/test.yml:
- Pinned actions/checkout@v1 → @50fbc622fc4ef5163becd7fab6573eac35f8462e # v1
- Added top-level permissions: { contents: read } (minimal permission needed to check out the repository)

