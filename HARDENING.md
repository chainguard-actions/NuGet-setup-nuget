<!-- markdownlint-disable -->

# Hardening Report: NuGet--setup-nuget/v2.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **NuGet--setup-nuget/v2.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow steps reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Failing references in codeql.yml: `actions/checkout@v6` (line 20), `github/codeql-action/init@v4` (line 33), `github/codeql-action/autobuild@v4` (line 39), `github/codeql-action/analyze@v4` (line 49). Failing reference in test.yml: `actions/checkout@v6` (line 14). All should be pinned to their full SHA digest, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/codeql.yml:20`
- `.github/workflows/codeql.yml:33`
- `.github/workflows/codeql.yml:39`
- `.github/workflows/codeql.yml:49`
- `.github/workflows/test.yml:14`

### missing-permissions (severity: medium)

Neither `.github/workflows/codeql.yml` nor `.github/workflows/test.yml` defines a top-level `permissions:` key, and neither job within those files defines a job-level `permissions:` key. Without explicit permissions, workflows run with the repository's default token permissions, which may be overly broad (e.g. write access to contents, pull-requests, etc.). Each workflow should declare minimal required permissions, e.g. `permissions: read-all` at the top level and then grant only what each job needs.

Locations:

- `.github/workflows/codeql.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references by resolving their full SHA hashes via lookup_action_sha: actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10, github/codeql-action/{init,autobuild,analyze}@v4 → @7188fc363630916deb702c7fdcf4e481b751f97a. Added top-level permissions blocks to both workflows: codeql.yml gets 'contents: read' and 'security-events: write' (required for CodeQL to upload SARIF results), test.yml gets 'contents: read' (minimal for checkout). Original version tags preserved as inline comments for readability.

