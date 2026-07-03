# Contributing to ICM Core Pipeline

> When a child project discovers something that could improve the ICM framework itself, this is the protocol for proposing that contribution back to the parent ICM repository.

## When to Contribute

You should consider submitting a contribution when, during your project's execution, you encounter:

- A **gap** in the current ICM template that caused confusion, failure, or extra work
- A **new stage pattern** that proved valuable and could benefit other projects
- An **error recovery strategy** that others might find useful
- A **formatting insight** that improves output quality across projects
- A **workflow optimization** that reduces token cost or time without sacrificing quality
- A **track variation** (lean/standard/rigorous) that works better for a domain

## When NOT to Contribute

Do **not** submit a contribution for:

- Project-specific logic (your domain knowledge, your data, your conclusions)
- Cosmetic changes (typos, formatting preferences that don't affect clarity)
- Changes that break backward compatibility without a clear migration path
- Features that only make sense for your specific project

## Contribution Workflow

### Step 1: Draft the Proposal

In your child project, create a file at:

```
{project-root}/icm-contribution-proposal.md
```

Use the template below. Fill in all fields.

### Step 2: Self-Validate

Before submitting, verify:

- [ ] The proposal addresses a **general** problem, not a project-specific one
- [ ] The change aligns with ICM's core goals (interpretable, stage-gated, human-in-the-loop)
- [ ] The change does not break backward compatibility (or includes a migration path)
- [ ] You have evidence: real project data, execution logs, comparison results
- [ ] The proposal includes a concrete diff or example

### Step 3: Submit

Create a pull request to the parent ICM repository with:

1. The proposal file (`icm-contribution-proposal.md`)
2. The proposed diff (actual file changes)
3. Evidence from your project execution (logs, state.md entries, before/after comparisons)

### Step 4: Review

The ICM maintainers (human) will evaluate using the criteria in `Review Criteria` below.

### Step 5: Resolution

| Outcome | Action |
|---------|--------|
| **Accepted** | Merged into next minor version. Your project credited in `CONTRIBUTIONS.md` and `release_notes/`. |
| **Conditional** | Merged with modifications. Your project credited. You'll be notified of changes. |
| **Deferred** | Valid but not priority. Logged in `proposals/accepted-d暂缓/` with rationale. May be revisited. |
| **Rejected** | Not merged. Reason documented in `proposals/rejected/` (anonymized if requested). |

## Proposal Template

```markdown
---
proposal_id: "PROP-{YYYY}-{NNN}"
submitted_by: "{child-project-name}"
submitted_date: "{ISO-8601}"
target_version: "{X.Y.Z}"
category: "stage-enhancement|config-enhancement|error-handling|formatting|new-feature|deprecation"
severity: "critical|major|minor|cosmetic"
backward_compatible: true|false
---

# Contribution Proposal: {Title}

## Problem Statement

{What problem did you encounter during your project execution? Be specific.}

## Evidence

{What data from your project execution supports this change? Include state.md entries, failure logs, before/after comparisons.}

## Proposed Change

{Describe the change to the ICM template. Include specific file paths and content.}

### Files Affected

- `{file_path}`: {what changes and why}

### Diff Preview

{Actual diff of proposed changes}

## Impact Analysis

- **Backward compatibility**: {Does this break existing child projects?}
- **Severity track impact**: {Does this affect lean/standard/rigorous differently?}
- **Token cost impact**: {More or fewer tokens required?}
- **Execution time impact**: {Faster or slower?}

## Recommendation

{What should the ICM maintainers do? Merge as-is, modify, defer, or reject? With reasoning.}
```

## Review Criteria

Proposals are evaluated on these dimensions (weighted):

| Criterion | Weight | Description |
|-----------|--------|-------------|
| **Generalizability** | 30% | Would this help OTHER projects, not just the submitter? |
| **Evidence Quality** | 25% | Is there real execution data supporting the change? |
| **Backward Compatibility** | 20% | Does this break existing child projects? |
| **Alignment with ICM Goals** | 15% | Does this improve interpretability, stage-gating, or human-in-the-loop? |
| **Implementation Cost** | 10% | How complex is it to merge and maintain? |

A proposal needs **≥70/100** to be accepted, with no single criterion below 15/100.

## Proposals Directory Structure

Accepted and pending proposals are tracked in:

```
icm-core-pipeline/proposals/
├── pending/          # Submitted but not yet reviewed
│   └── PROP-2026-001.md
├── accepted/         # Merged into a version
│   └── PROP-2026-001.md
├── deferred/         # Valid but not prioritized
│   └── PROP-2026-002.md
└── rejected/         # Declined with rationale
    └── PROP-2026-003.md
```

## Acknowledgments

All accepted contributors are listed in:

- `CONTRIBUTIONS.md` — The permanent ledger
- The release notes of the version where their contribution was merged
- The `IDENTITY.md` of the child project that contributed (credit embedded)
