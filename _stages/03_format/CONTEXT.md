---
source_stage: "03_format"
icm_core_version: "1.2.0"
lifecycle_status: "abstract-contract"
---
# 03_format — Format Stage

## Purpose

Take the Compiled Specification and produce the **Final Artifact** — a polished, publication-ready deliverable conforming to all formatting and structural standards.
This is the delivery stage: the artifact that the human ultimately consumes, shares, or deploys.

## Inputs

- [ ] `specification.md` from `_stages/02_compile/` (copied here during handoff)
- [ ] `self_review.md` (if in standard/rigorous track)
- [ ] `adversarial_review.md` (if in rigorous track)
- [ ] `IDENTITY.md` (version, track)
- [ ] `state.md` (execution log)
- [ ] Any external formatting standards (journal guidelines, company templates, style guides)

## Process

### Phase 1: Specification Review (Required — All Tracks)

1. Read the `specification.md` from the handoff input.
2. Map specification sections to the final artifact structure.
3. Identify any format-specific requirements (citation style, figure placement, page limits, file format).
4. Note any sections marked "needs human input" and flag for the human.

### Phase 2: Artifact Assembly (Required — All Tracks)

1. Produce the final artifact following the specification's structure.
2. Apply formatting standards from `_config/formatting.md#output-structure`.
3. Ensure all frontmatter is correct and complete.
4. Validate cross-references, citations, and internal links.
5. For code-based artifacts: ensure reproducibility (commands, dependencies, environment).

### Phase 3: Quality Check (Standard & Rigorous Only)

1. Apply the **Formatting Compliance Check**:
   - Does the artifact match the required output format exactly?
   - Are all frontmatter fields populated?
   - Are all cross-references valid?
   - Is the document structurally sound?
2. Apply the **Completeness Check**:
   - Does every specification section have a corresponding artifact section?
   - Are all decisions logged?
   - Are all open questions from earlier stages addressed or explicitly deferred?
3. Produce a **Quality Report** documenting compliance status.

### Phase 4: Final Polish (All Tracks)

1. Apply the chosen formatting standard (APA, IEEE, company template, custom).
2. Generate a **Table of Contents** or **Artifact Index**.
3. Produce the **Final Artifact** in the required delivery format.
4. If rigorous track: produce a **Delivery Summary** suitable for stakeholder review.

## Outputs

- **Final Artifact** — The primary output, ready for delivery.
  - Format: `{project_name}_final.md` (or stage-appropriate format) in `_stages/03_format/`
  - Must follow `_config/formatting.md#output-structure`
  - Must include frontmatter with `source_stage: "03_format"`

- **Quality Report** (Standard & Rigorous only)
  - Format: `quality_report.md` in `_stages/03_format/`

- **Delivery Summary** (Rigorous only)
  - Format: `delivery_summary.md` in `_stages/03_format/`

## Gate Criteria (What "Done" Looks Like)

| Criteria | Lean | Standard | Rigorous |
|----------|------|----------|----------|
| Final Artifact | Required | Required | Required |
| Formatting Compliance | Basic | Required | Required |
| Completeness Check | No | Required | Required |
| Quality Report | No | Required | Required |
| Delivery Summary | No | No | Required |
| Human checkpoint | No | Final | Final + review |

**Gate Pass Condition**: Final artifact exists, is non-empty, passes quality check (if applicable), and has been approved by human.

## Error Handling

- See `_config/rules.md#error-handling`
- If specification is missing or malformed: halt, generate `failure_report.md`, request human to review 02_compile output.
- If quality check fails: flag specific non-compliant sections, do NOT proceed without human decision.

## Handoff — Pipeline Complete

Upon gate pass:
1. Copy the final artifact to the project root or designated delivery location.
2. Update `state.md` with the final entry: status = `completed`.
3. Set `IDENTITY.md` `lifecycle_status` to `archived` (if project is complete) or keep as `active` (if iterations expected).
4. Generate a **Pipeline Completion Summary** in `state.md`:
   ```
   ## Pipeline Complete — {ISO-8601 timestamp}
   **Final artifact**: path/to/final_artifact.md
   **Track used**: lean|standard|rigorous
   **Stages completed**: 01_discover, 02_compile, 03_format
   **Human approved**: Yes/No
   **Notes**: {any final remarks}
   ```
5. Signal pipeline completion to human.

## Final Human Checkpoint

**DO NOT MARK COMPLETE** until the human has:
- Reviewed the Final Artifact
- Confirmed it meets their expectations
- Either archived the project or indicated iteration is needed

Human signals completion by explicit verbal/text confirmation or by creating `COMPLETED.txt` in `_stages/03_format/`.

## Contribution Opportunity (Self-Evolution Trigger)

During this stage's execution, if you discover any of the following, create a contribution proposal:

- A **formatting standard** that improves output quality across project types
- A **quality check technique** that catches issues other projects might miss
- A **delivery format** that proves more effective for stakeholder consumption
- A **template gap** that caused confusion during formatting

If found, create `{project-root}/icm-contribution-proposal.md` following the template in `CONTRIBUTING.md`. Do NOT modify the ICM template directly — proposals go through the PR process.
