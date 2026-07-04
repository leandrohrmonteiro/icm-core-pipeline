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

### Context Overflow Recovery Checklist

When context overflow is detected (agent stops, errors, or context feels lost):

- [ ] All state files committed to git before resuming
- [ ] `git log --oneline -10` reviewed for recent work
- [ ] Latest `state.md` for current stage read
- [ ] Stage `CONTEXT.md` reviewed for expected deliverables
- [ ] Resumed from last committed state
- [ ] Logged overflow in `CHANGELOG.md`

### Memory Consolidation Protocol

Uses git commits as natural "sleep cycles" to consolidate context, mirroring how the brain consolidates memories during sleep.

#### Active Context (Working Memory)

**File**: `session-summary.md` (created automatically at first commit)

**Content**:
- Current session ID
- Last committed state
- What was accomplished (bullet points)
- Key decisions (table)
- Blockers (list)
- Next steps (actionable)
- Git history (last 10 commits)

**When to Read**: At session start (replaces recovering from conversation history)
**When to Update**: Before context reaches 70% capacity

#### Consolidation (Sleep Cycle)

**Trigger**: Successful git commit of completed work

**Actions**:
1. Run `git log --oneline -5`
2. Append summary to `session-summary.md`:
   ```
   ## Session N — {timestamp}
   **Stage**: {stage name}
   **Status**: {completed/in-progress}
   **What was done**: {bullet points}
   **Key decisions**: {table}
   **Blockers**: {list}
   **For next stage**: {actionable}
   ```
3. If `session-summary.md` exceeds 500 lines:
   - Move oldest content to `archive/2026-07-04-session-N.md`
   - Keep only last 3 sessions in active file
4. Commit changes

**Frequency**: Every 5-10 git commits (or when context feels "heavy")

#### Archive (Long-Term Memory)

**Location**: `{project-root}/archive/`

**When to Load**: ONLY when:
- User explicitly requests archived information
- A trigger term is mentioned (e.g., "show me the archive", "recall session 1")
- Current context cannot resolve a question

**Do NOT Load**:
- Completed work that's no longer relevant
- Information already in current session-summary.md
- Work that's been superseded by newer commits

#### Retrieval (Cue-Triggered Recall)

**Trigger Terms**: "Show me the archive", "Recall session {N}", "What was done in {stage}?"

**Archive Index** (required for projects with >3 sessions):
- **File**: `{project-root}/archive/index.md`
- **Purpose**: Auto-generated table of contents enabling O(1) keyword search
- **Update**: After any archive operation (new session archived, session updated, session removed)
- **Example query**: User says "What authentication method was decided?" → Agent reads index.md, finds keyword, loads specific session file

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

Memory consolidation files (created automatically in child projects):
  {project-root}/session-summary.md  — Active working memory (always loaded)
  {project-root}/archive/            — Long-term archive directory
  {project-root}/archive/index.md    — Archive index (required for >3 sessions)
```
