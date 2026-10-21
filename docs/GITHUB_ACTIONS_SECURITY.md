<!-- SPDX-License-Identifier: MIT -->
# GitHub Actions Security Configuration

This document records the GitHub Actions security settings for the repository to ensure consistent configuration across environments and for audit purposes.

## Fork Pull Request Workflow Approval Policy

**Setting:** `all_external_contributors`

**Location:** Repository Settings → Actions → General → Fork pull request workflows

**Description:** Requires approval for all workflow runs triggered by pull requests from external forks, regardless of whether the contributor has previously contributed to the repository.

**Rationale:** Previously, the policy only required approval for first-time contributors. A compromised fork account that had previously been trusted could trigger dependency installation and builds automatically without approval. Setting the policy to `all_external_contributors` ensures that every external fork workflow run requires explicit approval, mitigating the risk of supply chain attacks via compromised contributor accounts.

**Impact:**
- ✅ Organization members retain ability to run workflows without approval
- ✅ Trusted internal collaborators (configured in organization settings) retain workflow access
- ✅ External fork workflows require approval for every run
- ✅ Fork workflows do not have access to repository secrets (default behavior preserved)

**Related Issue:** #487

**Configured Date:** 2026-10-21

---

## Workflow Permissions

All workflows in this repository are configured with minimal permissions:

```yaml
permissions:
  contents: read
```

This ensures workflows can only:
- Clone the repository
- Install dependencies
- Run tests

Workflows **cannot**:
- Push to the repository
- Comment on issues/PRs
- Publish packages
- Access repository secrets

**Reference:** `.github/workflows/*.yml`

---

## Secret Access Policy

Fork pull request workflows **never** receive access to repository secrets, regardless of approval status. This is a GitHub platform guarantee when the fork pull request workflow approval policy is enabled.

**Verified:** All workflows explicitly declare `contents: read` only, with no additional permissions that could expose secrets.