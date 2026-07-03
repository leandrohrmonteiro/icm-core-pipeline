# AGENTS.md — Agent Maintenance Protocol

## Purpose

Instructions for any LLM agent (Pi, Claude, GPT, etc.) on how to **maintain**, **execute**, and **evolve** this pipeline.

## Core Obligations

1. **Read `IDENTITY.md`** first. Confirm `icm_core_version` matches this template. If not, flag version mismatch.
2. **Never modify root `CONTEXT.md` or `_config/` directives** without explicit human authorization. These are the canonical source.
3. **Stage modifications** must preserve the `[Inputs]`, `[Process]`, `[Outputs]` placeholder structure and all sub-sections.
4. **Enforce the lifecycle loop**: ORIENT → CONTRACT → COMPILE → GATE → HANDOFF. No shortcuts.
5. **Do not generate code, text, or artifacts** outside of a targeted `_stages/XX_stage/` execution.
6. **Preserve semantic versioning** — structural changes increment `icm_core_version`.

## Self-Check Routines (Run Before Every Session)

Before executing any stage, verify:

- [ ] `IDENTITY.md` `icm_core_version` is current (matches template)
- [ ] `state.md` exists and is readable (create if missing: `_stages/<active_stage>/state.md`)
- [ ] Active stage's `CONTEXT.md` exists and is well-formed
- [ ] No prior incomplete work in the active stage folder (if so, review `state.md` for resume point)

## Error Recovery Protocol

When stuck or encountering errors:

1. **Log the issue** in `state.md` under the current stage.
2. **Read `_config/rules.md#ErrorHandling`** for applicable rules.
3. **Attempt one self-correction** using the error guidance.
4. **If still stuck**: Output a `failure_report.md` in the current stage folder and halt. Await human.
5. **Never** silently skip stages or fabricate outputs.

## Evolution Protocol

When a real project exposes gaps in this template:

1. Document the gap in `CHANGELOG.md` under `## Unresolved`.
2. Propose a fix to the human maintainer via `state.md` update.
3. **Do not** modify this template autonomously. Template changes require human sign-off.
4. When approved: increment `icm_core_version`, update `CHANGELOG.md`, and push the diff.

## Handoff Checklist

Before moving from one stage to the next:

- [ ] Output file exists in current stage folder
- [ ] Output file has valid `---` frontmatter with `source_stage`, `icm_core_version`, `timestamp`
- [ ] Output file passes content validation (non-empty, well-formed markdown)
- [ ] `state.md` records the completion
- [ ] Human has approved (explicit signal: file named `APPROVED.txt` in stage folder, or human confirmation in chat)

## File Path Reference

```
Root files:
  IDENTITY.md        — Version, lifecycle state, severity tracks
  CONTEXT.md         — Routing matrix, lifecycle protocol
  AGENTS.md          — This file: maintenance instructions
  PIPELINE.md        — State machine, stage topology, track definitions
  state.md           — Append-only execution log (created per project)
  CHANGELOG.md       — Version history
  CONTRIBUTING.md    — How child projects contribute back to ICM
  CONTRIBUTIONS.md   — Ledger of all accepted contributions

Config files:
  _config/rules.md           — Canonical source rules
  _config/formatting.md      — Structural styling rules

Contribution files:
  proposals/pending/         — Submitted proposals awaiting review
  proposals/accepted/        — Merged proposals (credited in CONTRIBUTIONS.md)
  proposals/deferred/        — Valid but not prioritized
  proposals/rejected/        — Declined with rationale
  release_notes/             — Per-version changelogs with attribution

Stage files:
  _stages/01_discover/CONTEXT.md     — Discover stage contract
  _stages/01_discover/state.md       — Discover execution log
  _stages/01_discover/*.md           — Output artifacts (created by execution)
  _stages/02_compile/CONTEXT.md      — Compile stage contract
  _stages/02_compile/state.md        — Compile execution log
  _stages/03_format/CONTEXT.md       — Format stage contract
  _stages/03_format/state.md         — Format execution log
```
