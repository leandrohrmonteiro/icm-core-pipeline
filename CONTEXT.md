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

## Agent Integration Layer

ICM's core pipeline is **harness-agnostic** — it defines the protocol (stages, gates, state management) but does not know about specific AI agents.

Agent-specific knowledge lives in **integration plugins** that sit between ICM and the harness:

### Architecture

```
┌─────────────────────────────────────────────────┐
│  ICM Core (harness-agnostic)                     │
│  • Stage protocol (discover → compile → format)  │
│  • State management (state.md, gates)            │
│  • File-based context management                  │
│  • Stress-tested via child projects               │
├─────────────────────────────────────────────────┤
│  Agent Integrations (plug-in layer)              │
│  • Pi integration: compaction hooks, token mon.  │
│  • Claude Code integration: (future)             │
│  • Other agents: (future)                        │
│  • Each knows: agent API, context window size    │
└─────────────────────────────────────────────────┘
```

### How It Works

1. **User picks an agent** (e.g., Pi, Claude Code)
2. **ICM loads the corresponding integration** (if available)
3. **Integration handles:**
   - Context window size (discovered or configured)
   - Compaction strategies (e.g., Pi's automatic compaction)
   - Token monitoring and overflow prevention
   - Agent-specific configuration (e.g., Pi's `ctx.compact()`)
4. **ICM core continues** — stages run, state tracked, gates enforced

### Current Integration: Pi

Pi provides:
- **Automatic compaction** when context approaches limits
- **Extension hooks**: `session_before_compact`, `session_compact`
- **Token monitoring**: `ctx.getContextUsage()`
- **Manual control**: `ctx.compact()`

Our Pi integration:
- Listens to compaction events
- Logs compaction to `state.md`
- Updates `session-summary.md` after compaction
- Provides data for memory consolidation decisions

### Future Integrations

When a child project uses a new agent:
1. Create an integration plugin (Pi extension pattern)
2. Document the agent's context management behavior
3. Add to `proposals/pending/` as a new-agent proposal
4. Tested via child project stress testing

### Design Principle

> ICM manages **what** gets presented to the model (files, stages, state).
> Agent integrations handle **how** each harness manages context overflow.
> The user picks an agent; ICM integrates with it automatically.
> The user should never need to know token counts.
