---
icm_core_version: "1.2.0"
lifecycle_status: "abstract-blueprint"
compatible_stages:
  - "01_discover"
  - "02_compile"
  - "03_format"
severity_tracks:
  - "lean"
  - "standard"
  - "rigorous"
changelog: "CHANGELOG.md"
contributions: "CONTRIBUTIONS.md"
contributing_guide: "CONTRIBUTING.md"
proposals_dir: "proposals/"
---
# ICM Core Pipeline Framework

This repository acts as the master abstract template for version **1.1.0**.

## Purpose

Child implementations **must** clone this folder structure and retain an explicit `icm_core_version` mapping to lock downstream compatibility.

## Lifecycle States

| State | Meaning | Transition Trigger |
|-------|---------|-------------------|
| `abstract-blueprint` | Template only — no active project | Initial / reset |
| `active` | Cloned into a live project | First stage execution |
| `archived` | Project completed or shelved | Human decision |
| `deprecated` | Superseded by newer `icm_core_version` | Manual |

## Versioning Policy

- **MAJOR** (X.0.0): Structural changes to pipeline (new stages, removed stages, restructured `_config/`)
- **MINOR** (x.Y.0): New content in stages without structural change
- **PATCH** (x.y.Z): Documentation, examples, or typo fixes

Any structural change to this template **requires** incrementing the version and updating `CHANGELOG.md`.

## Severity Tracks

Projects may select a complexity track that governs depth:

| Track | Stages Executed | Checkpoints | Output Rigor |
|-------|----------------|-------------|--------------|
| `lean` | All stages, minimal artifacts | 1 (final) | Minimum viable |
| `standard` | All stages + self-review | 2 (mid + final) | Documented |
| `rigorous` | All stages + adversarial review + cost audit | 3 (design, mid, final) | Production-grade |

Default track: `standard`. Override per-project in clone.

## Architecture: Two-Layer Design

ICM operates in two layers:

### Layer 1: ICM Core (Harness-Agnostic)

The protocol layer — defines **what** gets done:
- Stage topology (discover → compile → format)
- State management (state.md, gates, handoffs)
- File-based context management
- Severity tracks (lean/standard/rigorous)

This layer knows nothing about specific AI agents. It's the abstract blueprint.

### Layer 2: Agent Integrations (Plug-in)

The implementation layer — defines **how** each harness manages context:
- Context window size (discovered or configured)
- Compaction strategies (e.g., Pi's automatic compaction)
- Token monitoring and overflow prevention
- Agent-specific configuration (e.g., Pi's `ctx.compact()`)

**Example**: Pi integration handles compaction hooks, token monitoring, and session management via Pi's SDK.

### How Users Interact

1. User clones ICM core
2. User picks an agent (e.g., Pi)
3. ICM loads the corresponding integration (if available)
4. Integration handles agent-specific context management
5. ICM core runs the pipeline (stages, gates, state)

The user should never need to know token counts — the integration handles it.

## Self-Evolution Policy

ICM is designed to improve through its child projects' real-world executions.

When a child project discovers something that could strengthen the ICM framework:

1. The child project creates a contribution proposal (see `CONTRIBUTING.md`)
2. The proposal enters the review pipeline (`proposals/pending/`)
3. If accepted: merged into next version, credited in `CONTRIBUTIONS.md`
4. If rejected/deferred: documented in `proposals/` with rationale

**No contribution is too small.** Even a single project's error recovery insight can save dozens of future projects from the same pitfall.

**New**: Integration proposals (e.g., "add Claude Code integration") follow the same process — tested via child project stress testing, then merged into the next version.
