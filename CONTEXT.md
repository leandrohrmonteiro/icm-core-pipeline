# Global Context & Routing Matrix

This matrix governs how context layers are progressively loaded. It is the agent's **on-ramp** — read once, reference always.

## The Human-in-the-Loop Lifecycle Protocol

1. **ORIENT** — Read root `IDENTITY.md` and `CONTEXT.md`. Confirm `icm_core_version`. Check `lifecycle_status`.
2. **CONTRACT** — Navigate to the targeted active `_stages/XX_stage/` directory. Execute **ONLY** the `CONTEXT.md` instructions inside it. Nothing else.
3. **COMPILE** — Read only specified assets. Do **not** read the entire directory block. Use selective section routing (e.g., `rules.md#Syntax`).
4. **GATE** — Output an intermediate file to the stage folder. **STOP** execution and await human verification/editing.
5. **HANDOFF** — Once approved by a human, pass the output to the input directory of the next chronological stage.

## Routing Schema

```
Root (IDENTITY + CONTEXT)
  └── _config/          ← Canonical rules (loaded selectively via anchor)
      ├── rules.md
      └── formatting.md
  └── _stages/
      ├── 01_discover/
      │   └── CONTEXT.md   ← Enter here. Read only this file.
      ├── 02_compile/
      │   └── CONTEXT.md   ← Next. Read only this file.
      └── 03_format/
          └── CONTEXT.md   ← Last. Read only this file.
```

**Critical rule:** Each stage's `CONTEXT.md` is self-contained. Never read sibling stage files unless explicitly referenced.

## State Management

Every ICM project maintains a `state.md` (see [state.md](./state.md)) — an **append-only** log capturing:

- What stage is active
- What was produced
- Why specific choices were made
- What's blocked / awaiting human input

The agent **must** read `state.md` at ORIENT and update it at each GATE.

## Error Recovery Protocol

If a stage fails, times out, or produces invalid output:

1. **Log the failure** in `state.md` under the current stage.
2. **Check `severity_track`** from `IDENTITY.md`. If `lean`, skip to next stage with a warning note. If `standard` or `rigorous`, attempt **one retry** with narrowed scope.
3. **If retry fails**: Halt. Output a failure report to the stage folder. Signal human intervention.
4. **Do NOT** silently skip stages. Every skipped stage must be documented in `state.md` with reason.

## Cross-Stage Data Contract

| From Stage | To Stage | Data Handoff Format |
|-----------|----------|-------------------|
| 01_discover | 02_compile | Discovery document (markdown) |
| 02_compile | 03_format | Compiled specification (markdown) |
| 03_format | — | Final artifact (markdown or compiled) |

Each handoff file **must** include an `---` frontmatter block with `source_stage`, `icm_core_version`, and `timestamp`.
