# ICM Contributions Ledger

> A permanent record of all contributions accepted into the ICM Core Pipeline.
> Every child project that improves the framework is credited here.

## How Contributions Work

When a child project discovers something during execution that could improve the ICM framework:

1. The child project creates a contribution proposal (see `CONTRIBUTING.md`)
2. The proposal is reviewed by ICM maintainers
3. If accepted, the change is merged and this ledger is updated
4. The contributing project is credited in release notes

## Accepted Contributions

| # | Proposal ID | Project | Date | Category | Version Merged | Title |
|---|-------------|---------|------|----------|----------------|-------|
| — | — | — | — | — | — | _No contributions yet. The first child project will make history._ |

## Contribution Statistics

| Metric | Count |
|--------|-------|
| Total proposals received | 0 |
| Total accepted | 0 |
| Total deferred | 0 |
| Total rejected | 0 |
| Unique contributing projects | 0 |

## Proposals Directory

Active proposals are tracked in `proposals/`:

```
proposals/
├── pending/      # Submitted, awaiting review
├── accepted/     # Merged — credited here
├── deferred/     # Valid but not prioritized
└── rejected/     # Declined with rationale
```

## Release Notes

Per-version changelogs with attribution:

```
release_notes/
└── v{X.Y.Z}.md  — Changelog for version {X.Y.Z} with contribution credits
```
