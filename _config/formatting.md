# Formatting Rules

All output artifacts from ICM stages must conform to these structural standards.

## Section Anchors

| Anchor | Content | Used By |
|--------|---------|---------|
| `formatting.md#output-structure` | Required output file structure | 02_compile, 03_format |
| `formatting.md#frontmatter` | Required frontmatter fields | All stages |
| `formatting.md#state-log-format` | state.md entry format | state.md |
| `formatting.md#failure-report-structure` | failure_report.md structure | Error recovery |

## Frontmatter

Every ICM-generated file **must** include this frontmatter block:

```yaml
---
source_stage: "0X_stage_name"
icm_core_version: "X.Y.Z"
timestamp: "2026-07-03T12:00:00Z"
track: "lean|standard|rigorous"
---
```

### Field Definitions

| Field | Required | Description |
|-------|----------|-------------|
| `source_stage` | Yes | Stage ID that produced this file (e.g., `01_discover`) |
| `icm_core_version` | Yes | Version of ICM template used (read from `IDENTITY.md`) |
| `timestamp` | Yes | ISO-8601 UTC timestamp of creation |
| `track` | Yes | Severity track used for this execution |

## Output Structure

Every stage output file must follow this structure:

```markdown
---
{frontmatter block}
---

# {Stage Name} — {Project Topic}

## Summary
{1-3 sentence overview of what this artifact contains}

## Core Content
{Stage-specific deliverable}

## Decisions Log
| # | Decision | Rationale | Stage |
|---|----------|-----------|-------|
| 1 | ... | ... | ... |

## Open Questions
- {unresolved items requiring human input}

## Next Stage Guidance
{Specific instructions for the next stage's agent}
```

### Stage-Specific Variations

| Stage | "Core Content" becomes | "Next Stage Guidance" becomes |
|-------|----------------------|------------------------------|
| 01_discover | Discovery Document | "Compile Focus Areas" |
| 02_compile | Compiled Specification | "Format Requirements" |
| 03_format | Final Artifact | "Delivery Instructions" |

## State Log Format

Entries in `state.md` follow this pattern:

```markdown
## S{N}_{stage_id} — {stage_name} — {ISO-8601 timestamp}

**Stage**: 0X_<name>
**Track**: lean | standard | rigorous
**Status**: active | completed | skipped | failed
**Output**: path/to/output.md (or "none")

**What was done**: {1-3 sentence summary}
**Key decisions**:
- {decision 1}: {rationale}
- {decision 2}: {rationale}

**What changed**: {how this altered project direction or method}

**Blockers**: {any blocking issues, or "none"}

**For next stage**: {specific guidance for the next stage's agent}

**Human review**: {approved | rejected | pending | N/A}
```

## Failure Report Structure

When a stage fails, `failure_report.md` must contain:

```markdown
---
source_stage: "0X_stage_name"
icm_core_version: "X.Y.Z"
timestamp: "ISO-8601"
track: "lean|standard|rigorous"
type: "failure_report"
---

# Failure Report: {stage_name}

## What Was Attempted
{Description of the stage execution and what it was trying to produce}

## What Failed
{Specific failure point, error, or condition}

## What Was Tried
{Any self-correction attempts, with results}

## Current Blockers
- {list of things blocking progress}

## Recommendation
{What should happen next: retry, skip, escalate to human, etc.}

## Human Action Required
{Specific instructions for the human maintainer}
```
