# Canonical Source Rules

All rules defined here are **authoritative**. Stage folders MUST reference these via anchor links — never duplicate.

## Content Rules

### Clarity
- Write for an LLM agent, not a human. Be explicit, unambiguous, and actionable.
- Use imperative mood ("Read X. Do Y. Output Z.").
- Avoid metaphor, humor, or ambiguous phrasing.

### Scope
- Each file has a defined scope. Do not inject out-of-scope metadata into active token space.
- If a rule belongs in multiple stages, put it here. Reference via `rules.md#section-name` (lowercase, hyphenated).
- If a rule is stage-specific, put it in that stage's `CONTEXT.md`.

### Versioning
- All outputs from ICM stages must include a frontmatter block:
  ```yaml
  ---
  source_stage: "0X_stage_name"
  icm_core_version: "X.Y.Z"
  timestamp: "ISO-8601"
  track: "lean|standard|rigorous"
  ---
  ```
- Never hardcode a version — read from `IDENTITY.md`.

## Error Handling

### Failure Protocol
1. Log failure in `state.md` under the relevant stage.
2. Check severity track for retry policy.
3. Generate `failure_report.md` with: what was attempted, what failed, what was tried, what's blocked.
4. Halt. Signal human.

### Silent Failure Prevention
- **Never** skip a stage without documenting the skip in `state.md` with explicit reason.
- **Never** fabricate output. If a stage cannot produce valid output, halt.
- **Always** create `failure_report.md` in the stage folder when halting.

### Timeout Handling
- If a stage exceeds reasonable bounds: log in `state.md`, reduce scope, retry once.
- If retry fails: halt with `failure_report.md`.

## Context Overflow Recovery Protocol

When an LLM agent detects or recovers from a context window overflow:

### Immediate Actions
1. **Stop all work** — Do not attempt to continue in overflowed context
2. **Commit all state** — Ensure `state.md`, `discovery.md`, `specification.md` etc. are committed
3. **Log the overflow** — Add entry to project's `CHANGELOG.md`:
   ```
   ## [Context Overflow] — {timestamp}
   - Session: {session-id}
   - Work lost: {what was being done}
   - Recovery method: {how it was recovered}
   ```

### Recovery Procedure
1. Check `git log --oneline -10` for recent commits
2. Read the latest `state.md` for the current stage
3. Read the stage's `CONTEXT.md` to understand expected work
4. Resume from the last committed state

### Prevention Strategies
1. **Small commits**: Commit after every logical unit of work (not just at stage end)
2. **State files**: Write progress to `state.md` files, not just conversation
3. **Atomic operations**: Don't read multiple large files in one response
4. **Checksum verification**: After overflow, verify file integrity with `git diff`

### Agent Guidelines
- If a response feels "long" or you're reading many files, pause and commit
- Before starting new work, run `git log --oneline -5` to confirm state
- If you cannot recover context from git + state files, halt and request human intervention

## Validation Rules

### Handoff Validation
Every file passed between stages MUST:
- [ ] Exist and be non-empty (>100 bytes)
- [ ] Contain valid `---` frontmatter with `source_stage`, `icm_core_version`, `timestamp`
- [ ] Be well-formed markdown (parseable headings, no broken anchor references)
- [ ] Contain no `TODO`, `FIXME`, or placeholder text (unless explicitly allowed)

### Self-Check (Pre-Stage)
Before executing any stage, verify:
- [ ] `IDENTITY.md` is readable and version matches template
- [ ] `state.md` exists and is current
- [ ] Previous stage's output file exists (if not first stage)
- [ ] Active stage's `CONTEXT.md` exists and is well-formed

## Anchor Links

These anchors are referenced by stage CONTEXT.md files (format: `filename#heading-name`):

| Anchor | Section | Used By |
|--------|---------|---------|
| `rules.md#content-rules` | Content clarity/scope/versioning | All stages |
| `rules.md#error-handling` | Failure, timeout, retry protocols | All stages |
| `rules.md#context-overflow-recovery` | Overflow detection, recovery, prevention | All stages |
| `rules.md#validation-rules` | Handoff and self-check validation | All stages |
| `formatting.md#output-structure` | Output file structure | 02_compile, 03_format |
| `formatting.md#frontmatter` | Required frontmatter fields | All stages |
