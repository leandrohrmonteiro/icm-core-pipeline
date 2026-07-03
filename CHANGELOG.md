# CHANGELOG — ICM Core Pipeline

All notable changes to this template are documented here.

## [Unresolved]

_Gaps discovered during real project implementations. Track issues here before proposing fixes._

- [ ] No validation schema for handoff artifacts (machine-readable format spec)
- [ ] No guidance for projects with >3 stages (pipeline extension)
- [ ] No automated proposal reviewer (could be an LLM task)
- [ ] No cross-project contribution aggregation (trend analysis across child projects)

## [1.2.0] — 2026-07-03

### Added
- `CONTRIBUTING.md` — Full contribution protocol for child projects proposing changes back to ICM
- `CONTRIBUTIONS.md` — Permanent ledger tracking all accepted contributions with attribution
- `proposals/` directory — Structured proposal pipeline (pending/accepted/deferred/rejected)
- `release_notes/` directory — Per-version changelogs with contribution credits
- **Self-Evolution Policy** in `IDENTITY.md` — ICM improves through child project real-world executions
- **Contribution Opportunity triggers** in all 3 stage `CONTEXT.md` files — Agents self-detect improvement opportunities during execution
- Per-stage `state.md` templates (01_discover, 02_compile, 03_format)
- Proposal review criteria (5-dimension weighted scoring, ≥70/100 acceptance threshold)

### Changed
- `IDENTITY.md` — Upgraded to v1.2.0, added contribution metadata, self-evolution policy
- `AGENTS.md` — Updated file path reference to include contribution system
- All stage `CONTEXT.md` files — Added contribution opportunity triggers

### Fixed
- All cross-reference anchor links corrected to proper markdown heading format (`#heading-name`)
- Mixed Chinese characters removed from `_config/rules.md`

## [1.1.0] — 2026-07-03

### Added
- `PIPELINE.md` — State machine definition with stage topology and track definitions
- `state.md` — Append-only execution state log (root level)
- `AGENTS.md` — Expanded with self-check routines, error recovery, evolution protocol, handoff checklist
- `IDENTITY.md` — Added lifecycle state machine, severity tracks (lean/standard/rigorous), versioning policy
- `CONTEXT.md` — Added routing schema, state management, error recovery protocol, cross-stage data contract

### Changed
- `IDENTITY.md` — Upgraded from v1.0.0 to v1.1.0 (structural addition of state machine and tracks)
- `CONTEXT.md` — Expanded from basic lifecycle to include routing, state, error recovery, data contracts

### Fixed
- Version bump reflects structural changes (MAJOR version policy now documented)

## [1.0.0] — 2026-07-03

- Initial abstract blueprint
- 3-stage pipeline (discover → compile → format)
- Root-level context and stage contracts
