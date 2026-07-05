# ICM Pipeline Definition

> This file defines the state machine for ICM execution. Read this once during ORIENT to understand the full pipeline topology.

## Architecture: Two-Layer Design

ICM operates in two layers to separate protocol from implementation:

```
┌─────────────────────────────────────────────────────────────────────────┐
│  ICM Core (Harness-Agnostic)                                           │
│  ─────────────────────────────                                         │
│  • Stage topology (discover → compile → format)                        │
│  • State management (state.md, gates, handoffs)                        │
│  • File-based context management                                       │
│  • Stress-tested via child projects                                    │
│                                                                         │
│  Manages **what** gets presented to the model (files, stages, state).   │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│  Agent Integrations (Plug-in Layer)                                    │
│  ─────────────────────────────────────                                 │
│  • Pi integration: compaction hooks, token monitoring                   │
│  • Claude Code integration: (future)                                   │
│  • Other agents: (future)                                              │
│                                                                         │
│  Handles **how** each harness manages context overflow (API, tokens).   │
└─────────────────────────────────────────────────────────────────────────┘
```

### Design Principle

> ICM's core is harness-agnostic — it defines the protocol.
> Agent integrations are plug-ins that know about specific agents.
> The user picks an agent; ICM integrates with it automatically.
> The user should never need to know token counts.

## Stage Topology

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  ORIENT (IDENTITY.md + CONTEXT.md)                                        │
│       │                                                                     │
│       ▼                                                                     │
│  01_discover — [Human check at P0]                                         │
│       │                                                                     │
│       ▼                                                                     │
│  02_compile — [Human check at S2]                                          │
│       │                                                                     │
│       ▼                                                                     │
│  03_format — [Final checkpoint]                                            │
│       │                                                                     │
│       ▼                                                                     │
│  ARCHIVED / handoff to production                                           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Stage Map

| Stage ID | Directory | Purpose | Severity Tracks | Checkpoint |
|----------|-----------|---------|----------------|------------|
| `01_discover` | `_stages/01_discover/` | Research, scope, identify problem | All | Yes (P0: direction) |
| `02_compile` | `_stages/02_compile/` | Synthesize, design solution | `standard`, `rigorous` | Yes (S2: design) |
| `03_format` | `_stages/03_format/` | Format, validate, deliver | All | Yes (final) |

## Track Definitions

### `lean` Track (Minimum Viable)

- All 3 stages executed
- 1 checkpoint (final only)
- No self-review
- Output: Minimum viable artifacts
- Use when: Rapid prototyping, internal exploration

### `standard` Track (Default)

- All 3 stages executed
- 2 checkpoints (mid + final)
- Includes self-review pass
- Output: Documented artifacts
- Use when: Most projects

### `rigorous` Track

- All 3 stages executed
- 3 checkpoints (design, mid, final)
- Includes adversarial review
- Cost-attributed audit trail
- Output: Production-grade artifacts
- Use when: External delivery, compliance, high-stakes

## Execution Rules

1. **Sequential only.** No parallel stage execution. Each stage's output is the next stage's input.
2. **Human gate is mandatory.** Pipeline MUST stop at each checkpoint. No autonomous advancement.
3. **State tracking is mandatory.** `state.md` must exist and be updated at every transition.
4. **Failure halts pipeline.** A failed stage does NOT auto-skip (unless `lean` track with explicit warning).

## Handoff Protocol

When transitioning between stages:

```
[Stage N Output]
  → frontmatter: { source_stage: "0N_stage", icm_core_version: "X.Y.Z", timestamp: "ISO-8601" }
  → target: _stages/0N+1_stage/ (copied as input, not moved)
  → state.md updated: { stage: "0N_stage", status: "completed", output: "filename.md", timestamp: "..." }
```
