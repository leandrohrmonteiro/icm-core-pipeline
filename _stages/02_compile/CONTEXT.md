---
source_stage: "02_compile"
icm_core_version: "1.2.0"
lifecycle_status: "abstract-contract"
---
# 02_compile — Compile Stage

## Purpose

Take the Discovery Document and synthesize it into a **Compiled Specification** — a concrete, actionable plan for the final artifact.
This stage translates "what we know" into "what we will build" with specific decisions, architecture, and validation criteria.

## Inputs

- [ ] `discovery.md` from `_stages/01_discover/` (copied here during handoff)
- [ ] `IDENTITY.md` (version, track)
- [ ] `state.md` (execution log)
- [ ] Any domain-specific references required by the discovery

## Process

### Phase 1: Ingestion (Required — All Tracks)

1. Read the `discovery.md` from the handoff input.
2. Extract and index: core question, constraints, knowns, unknowns, assumptions, selected direction.
3. Map unknowns to specific questions that must be answered in the final artifact.
4. Validate: does the discovery document have all required sections? If not, note gaps.

### Phase 2: Specification Design (Required — All Tracks)

1. Define the **final artifact type** (paper, report, code, design doc, etc.).
2. Define the **structure** of the final artifact (sections, subsections, required elements).
3. For each section, specify:
   - What information must appear
   - What source(s) feed this section (from discovery)
   - What validation criteria apply
4. Produce a **Specification Outline** with section-by-section requirements.

### Phase 3: Drafting (Required — All Tracks)

1. For each section in the specification:
   - Synthesize content from discovery + any additional research
   - Flag assumptions that need human validation
   - Note where data is missing and propose resolution
2. Maintain a **Decisions Log** throughout: every design choice with rationale.

### Phase 4: Self-Review (Standard & Rigorous Only)

1. After initial draft: revisit each section.
2. Apply the **Consistency Check**:
   - Does every claim trace back to the discovery document?
   - Are all assumptions from 01_discover addressed?
   - Is the structure internally consistent?
3. Apply the **Gap Analysis**:
   - What is missing that would strengthen the artifact?
   - What assumptions remain unvalidated?
4. Produce a **Self-Review Report** documenting findings.
5. Revise the specification based on self-review.

### Phase 5: Adversarial Review (Rigorous Only)

1. Generate 2-3 counter-arguments or alternative interpretations of the discovery.
2. Test the specification against each counter-argument.
3. Document weaknesses and fortify the specification where possible.
4. Produce an **Adversarial Review Report**.

## Outputs

- **Compiled Specification** — The primary output.
  - Format: `specification.md` in `_stages/02_compile/`
  - Must follow `_config/formatting.md#output-structure`
  - Must include frontmatter with `source_stage: "02_compile"`

- **Self-Review Report** (Standard & Rigorous only)
  - Format: `self_review.md` in `_stages/02_compile/`

- **Adversarial Review Report** (Rigorous only)
  - Format: `adversarial_review.md` in `_stages/02_compile/`

## Gate Criteria (What "Done" Looks Like)

| Criteria | Lean | Standard | Rigorous |
|----------|------|----------|----------|
| Specification Document | Required | Required | Required |
| Section-by-section mapping from discovery | Required | Required | Required |
| Decisions Log | Minimal | Required | Required + traceable |
| Self-Review Report | No | Required | Required |
| Adversarial Review | No | No | Required |
| Human checkpoint | No | Yes (S2) | Yes (S2) |

**Gate Pass Condition**: Specification document exists, is non-empty, passes self-review (if applicable), and has been reviewed by human.

## Error Handling

- See `_config/rules.md#error-handling`
- If discovery document is missing or malformed: halt, generate `failure_report.md`, request human provide valid discovery.
- If specification cannot be derived from discovery: halt, generate `failure_report.md` with gap analysis.

## Handoff to 03_format

Upon gate pass:
1. Copy `specification.md` from `_stages/02_compile/` → `_stages/03_format/inputs/`
2. If in standard/rigorous track, also copy `self_review.md` and/or `adversarial_review.md`.
3. Update `state.md` with this stage's completion entry.
4. Confirm human approval (check for `APPROVED.txt` in stage folder or explicit confirmation).
5. Signal readiness for 03_format.

## Human Checkpoint (S2)

**DO NOT PROCEED** to 03_format until the human has:
- Read the Compiled Specification
- Validated the section structure
- Approved self-review findings (if in standard/rigorous track)
- Confirmed readiness for final formatting

Human signals approval by creating `APPROVED.txt` in `_stages/02_compile/` or explicit verbal/text confirmation.

## Contribution Opportunity (Self-Evolution Trigger)

During this stage's execution, if you discover any of the following, create a contribution proposal:

- A **synthesis pattern** that produces better specifications than the template suggests
- A **self-review technique** that catches more issues
- A **specification structure** that reduces downstream formatting errors
- An **error during compilation** that reveals a template gap

If found, create `{project-root}/icm-contribution-proposal.md` following the template in `CONTRIBUTING.md`. Do NOT modify the ICM template directly — proposals go through the PR process.
