<!-- markdownlint-disable -->

# Hardening Report: NuGet--setup-nuget/v4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **NuGet--setup-nuget/v4.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in .github/workflows/codeql.yml are pinned to mutable version tags (@v6, @v4) rather than immutable 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the upstream action tag is moved or compromised. Failing references: `actions/checkout@v6` (line 24), `github/codeql-action/init@v4` (line 27), `github/codeql-action/analyze@v4` (line 35).

Locations:

- `.github/workflows/codeql.yml:24`
- `.github/workflows/codeql.yml:27`
- `.github/workflows/codeql.yml:35`

### unpinned-uses (severity: high)

The `uses:` reference in .github/workflows/test.yml is pinned to a mutable version tag (@v6) rather than an immutable 40-character SHA commit hash. This exposes the workflow to supply-chain attacks if the upstream action tag is moved or compromised. Failing reference: `actions/checkout@v6` (line 12).

Locations:

- `.github/workflows/test.yml:12`

### missing-permissions (severity: medium)

.github/workflows/test.yml has no top-level `permissions:` key and its only job (`test`) also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad (e.g., write access to contents). A minimal permissions block should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings: (1) In codeql.yml, pinned actions/checkout@v6 to SHA d23441a48e516b6c34aea4fa41551a30e30af803 and both github/codeql-action/init@v4 and github/codeql-action/analyze@v4 to SHA e4fba868fa4b1b91e1fdab776edc8cfbe6e9fb81. (2) In test.yml, pinned actions/checkout@v6 to SHA d23441a48e516b6c34aea4fa41551a30e30af803. (3) In test.yml, added a top-level `permissions: {}` block to restrict the GITHUB_TOKEN to no permissions by default, satisfying the least-privilege requirement.

