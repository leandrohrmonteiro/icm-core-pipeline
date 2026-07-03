---
source_stage: "01_discover"
icm_core_version: "1.2.0"
lifecycle_status: "abstract-contract"
---
# 01_discover — Discover Stage

## Purpose

Explore the problem space. Identify scope, constraints, stakeholders, and the core question.
This stage produces the **Discovery Document** — the foundation upon which all subsequent stages build.

## Inputs

- [ ] Project topic or problem statement (provided by human)
- [ ] Any existing documentation, code, or data relevant to the problem
- [ ] `IDENTITY.md` (for version and track configuration)
- [ ] `state.md` (for prior state, if resuming)
- [ ] Previous stage output (if resuming from 01_discover)

## Process

### Phase 1: Scoping (Required — All Tracks)

1. Read the project topic/problem statement provided by the human.
2. Identify **constraints** (time, resources, technology, domain).
3. Identify **stakeholders** and their expectations.
4. Formulate the **core question** in one sentence.
5. Produce a **Scope Statement**: `{topic} + {constraints} + {stakeholders} = {core question}`

### Phase 2: Research (Required — All Tracks)

1. Map the problem space: what is known, what is unknown, what is assumed.
2. Identify **knowns** (facts, data, established methods).
3. Identify **unknowns** (gaps, unanswered questions).
4. Identify **assumptions** (things taken for granted — flag for human validation).
5. Produce a **Discovery Document** with sections: Knowns, Unknowns, Assumptions, Constraints.

### Phase 3: Direction Alignment (Required — Standard & Rigorous Tracks)

1. Propose 2-3 candidate research directions or solution approaches.
2. For each direction, note: feasibility, risk, expected effort, alignment with constraints.
3. Present for human selection. **DO NOT proceed** until human picks a direction.

### Phase 4: Validation (Standard & Rigorous Only)

1. Adversarially challenge your own discoveries:
   - "What if this assumption is wrong?"
   - "What is the simplest possible version of this problem?"
   - "What could go wrong?"
2. Document the challenges and your responses.
3. Refine the Discovery Document based on adversarial review.

## Outputs

- **Discovery Document** — The primary output of this stage.
  - Format: `discovery.md` in `_stages/01_discover/`
  - Must follow `_config/formatting.md#output-structure`
  - Must include frontmatter with `source_stage: "01_discover"`

## Gate Criteria (What "Done" Looks Like)

| Criteria | Lean | Standard | Rigorous |
|----------|------|----------|----------|
| Scope Statement | Required | Required | Required |
| Discovery Document | Required | Required | Required + adversarial |
| Candidate Directions | Optional | Required (2-3) | Required (3) + risk matrix |
| Human checkpoint | No | Yes (P0) | Yes (P0) |

**Gate Pass Condition**: Discovery Document exists, is non-empty, and has been reviewed by human.

## Error Handling

- See `_config/rules.md#error-handling`
- If human does not respond within timeout: log in `state.md`, reduce to `lean` outputs, continue with warning.
- If no valid discovery can be produced: halt, generate `failure_report.md`, await human.

## Handoff to 02_compile

Upon gate pass:
1. Copy `discovery.md` from `_stages/01_discover/` → `_stages/02_compile/inputs/`
2. Update `state.md` with this stage's completion entry.
3. Confirm human approval (check for `APPROVED.txt` in stage folder or explicit confirmation).
4. Signal readiness for 02_compile.

## Human Checkpoint (P0)

**DO NOT PROCEED** to 02_compile until the human has:
- Read the Discovery Document
- Validated or adjusted the Scope Statement
- Selected or rejected candidate directions (if in standard/rigorous track)

Human signals approval by creating `APPROVED.txt` in `_stages/01_discover/` or explicit verbal/text confirmation.

## Contribution Opportunity (Self-Evolution Trigger)

During this stage's execution, if you discover any of the following, create a contribution proposal:

- A **gap** in the ICM template that caused confusion or extra work
- A **new pattern** that proved valuable and could help other projects
- An **error recovery strategy** that worked better than the template suggests
- A **formatting insight** that improved output quality

If found, create `{project-root}/icm-contribution-proposal.md` following the template in `CONTRIBUTING.md`. Do NOT modify the ICM template directly — proposals go through the PR process.
