# ICM Core Pipeline Framework

**Version:** 1.2.0  
**Status:** Active — Abstract Blueprint  
**License:** MIT (implied by open source)

---

## What is ICM?

The **Interpretable Context Methodology (ICM) Core Pipeline** is a versioned, abstract blueprint system for LLM agent workflows. It provides a strict, stage-gated execution framework where individual projects inherit this exact directory structure and lifecycle loop.

ICM is designed for **LLM agents** as the primary reader, with humans as verified checkpoints. Every file is written to be unambiguous, actionable, and executable by an AI agent.

## Key Features

- ✅ **3-Stage Pipeline:** discover → compile → format
- ✅ **Severity Tracks:** lean (minimal), standard (documented), rigorous (production-grade)
- ✅ **Human-in-the-Loop:** Mandatory checkpoints at each stage
- ✅ **Self-Evolution:** Child projects can contribute improvements back to the framework
- ✅ **Error Recovery:** Built-in failure handling, retry logic, and state tracking
- ✅ **State Management:** Append-only execution logs enable resume after interruption

## Repository Structure

```
icm-core-pipeline/
├── IDENTITY.md              — Version, lifecycle state, severity tracks
├── CONTEXT.md               — Routing matrix, lifecycle protocol
├── AGENTS.md                — Maintenance, evolution, handoff instructions
├── PIPELINE.md              — State machine, stage topology, track definitions
├── CHANGELOG.md             — Version history (v1.0 → v1.2)
├── CONTRIBUTING.md          — Contribution protocol & proposal template
├── CONTRIBUTIONS.md         — Permanent contributions ledger
├── state.md                 — Root execution log (template)
├── _config/
│   ├── rules.md             — Canonical rules + anchor index
│   └── formatting.md        — Formatting standards + templates
├── _stages/
│   ├── 01_discover/
│   │   ├── CONTEXT.md       — Discover stage contract (13 sections)
│   │   └── state.md         — Per-stage execution log
│   ├── 02_compile/
│   │   ├── CONTEXT.md       — Compile stage contract (14 sections)
│   │   └── state.md
│   └── 03_format/
│       ├── CONTEXT.md       — Format stage contract (14 sections)
│       └── state.md
├── proposals/
│   ├── pending/             — Submitted proposals awaiting review
│   ├── accepted/            — Merged proposals (credited)
│   ├── deferred/            — Valid but not prioritized
│   └── rejected/            — Declined with rationale
└── release_notes/           — Per-version changelogs with attribution
```

## Quick Start

### 1. Clone the Template

```bash
git clone https://github.com/leandrohrmonteiro/icm-core-pipeline.git my-project
cd my-project
```

### 2. Select Your Track

Edit `IDENTITY.md` and set your severity track:

| Track | Best For | Checkpoints | Output Rigor |
|-------|----------|-------------|--------------|
| `lean` | Rapid prototyping | 1 (final) | Minimum viable |
| `standard` (default) | Most projects | 2 (mid + final) | Documented |
| `rigorous` | External delivery | 3 (design, mid, final) | Production-grade |

### 3. Execute the Pipeline

1. **ORIENT** — Read `IDENTITY.md` and `CONTEXT.md`
2. **CONTRACT** — Navigate to the active stage's `CONTEXT.md`
3. **COMPILE** — Execute ONLY that stage's instructions
4. **GATE** — Output intermediate file, await human verification
5. **HANDOFF** — Once approved, pass output to next stage

Each stage's `CONTEXT.md` is self-contained. Read only what's needed.

## How It Works

### The Lifecycle Loop

```
ORIENT → CONTRACT → COMPILE → GATE → HANDOFF
   ↑                              │
   └──────── (on failure) ─────────┘
```

### Stage Topology

```
┌─────────────────────────────────────────────────────────────┐
│  ORIENT (IDENTITY.md + CONTEXT.md)                         │
│       │                                                     │
│       ▼                                                     │
│  01_discover — [Human check at P0]                         │
│       │                                                     │
│       ▼                                                     │
│  02_compile — [Human check at S2]                          │
│       │                                                     │
│       ▼                                                     │
│  03_format — [Final checkpoint]                            │
│       │                                                     │
│       ▼                                                     │
│  ARCHIVED / handoff to production                           │
└─────────────────────────────────────────────────────────────┘
```

### Human Checkpoints

At each stage, the pipeline **STOPS** and awaits human approval:

- **P0 (discover):** Validate scope, select direction
- **S2 (compile):** Validate specification, approve self-review
- **Final (format):** Approve final artifact

Human signals approval by creating `APPROVED.txt` in the stage folder or explicit confirmation.

## Self-Evolution

ICM improves through its child projects' real-world executions.

When a child project discovers something valuable during execution:

1. Create `icm-contribution-proposal.md` (see `CONTRIBUTING.md`)
2. Submit a PR to this repository
3. If accepted: merged into next version, credited in `CONTRIBUTIONS.md`

**No contribution is too small.** Even a single project's error recovery insight can save dozens of future projects from the same pitfall.

## For Agents (LLMs)

This framework is designed for LLM agents as the primary executor. Key principles:

- **Write for agents, not humans.** Be explicit, unambiguous, actionable.
- **Use imperative mood.** "Read X. Do Y. Output Z."
- **Reference via anchors.** `rules.md#error-handling`, not repeated rules.
- **Respect scope.** Each file has a defined boundary.
- **Honor checkpoints.** Never advance without human approval.

## Version History

### [1.2.0] — 2026-07-03

- **Added:** `CONTRIBUTING.md`, `CONTRIBUTIONS.md`, `proposals/` directory, `release_notes/`
- **Added:** Self-Evolution Policy, contribution triggers in all stages
- **Added:** Per-stage `state.md` templates
- **Fixed:** Cross-reference anchor links, mixed characters removed

### [1.1.0] — 2026-07-03

- **Added:** `PIPELINE.md`, `state.md`, expanded `AGENTS.md`, `IDENTITY.md`, `CONTEXT.md`
- **Added:** Lifecycle state machine, severity tracks, error recovery, data contracts

### [1.0.0] — 2026-07-03

- Initial abstract blueprint
- 3-stage pipeline (discover → compile → format)

Full changelog: `CHANGELOG.md`

## Contributing

See `CONTRIBUTING.md` for the full contribution protocol.

**TL;DR:**
1. Encounter a gap, pattern, or improvement during execution?
2. Draft a proposal using the template in `CONTRIBUTING.md`
3. Submit a PR with the proposal + evidence from your project
4. If accepted: merged, version-bumped, credited in `CONTRIBUTIONS.md`

## License

This is an open-source framework. Use it, clone it, improve it.

---

**Ready to run your first project?** Clone this repository and begin with `IDENTITY.md`.
