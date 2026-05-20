# Research And Plan Review

Project: `trino-policy-receipts`

## Refined Thesis

Agentic SQL over federated data needs proof that governed data products constrain discovery, access, and generated queries.

The implementation is intentionally local and synthetic, but the test harness is shaped around the real operating question: can the proposed artifact create evidence a founder, CTO, or data-platform leader would immediately recognize as useful?

## Fresh Sources Checked

- Public Trino security and access-control documentation.
- Local synthetic governed-query fixture model.

## Plan Excerpt Used

## The Gap

Data agents can generate SQL quickly, but governed data products need proof that every generated query respects row-level policy, column masking, catalog boundaries, and semantic scope. A passing answer is not enough; teams need receipts showing which policy constrained which action.

The missing artifact is a local, reproducible governance-first harness for Trino-backed agent workflows. It should show the query, the policy context, the lineage, the blast radius, and whether sensitive columns or cross-catalog joins were attempted.

## The Project - `trino-policy-receipts`

> A local governed-query receipt generator for Trino-style AI data agents, focused on policy adherence, query blast radius, lineage coverage, and Iceberg-context fit.

**What it is.** A deterministic CLI, fixture generator, evaluator, dashboard, benchmark, and evidence-pack exporter. It models data products, user roles, governed joins, sensitive columns, lineage coverage, and generated-query drift.

**Why it solves the gap.** Three vectors:

1. **Policy receipts.** Every agent action gets an inspectable receipt rather than a vague pass/fail label.
2. **Blast-radius scoring.** Unsafe joins, cross-catalog leaks, and sensitive-column touches are first-class failure modes.
3. **Portable governance proof.** The generated evidence can be reviewed without connecting to production data or a live warehouse.

**The demonstration moment.** A synthetic governed data product receives several generated queries. The dashboard marks which queries stayed inside policy, which attempted ungoverned joins, and which touched sensitive columns. The evidence pack gives a reviewer the exact reason each query was allowed or blocked.

## Prototype Plan

- **Fixture generator:** deterministic clean and degraded governed-query cases.
- **Evaluator:** scores policy adherence, query blast radius, lineage coverage, and Iceberg-context fit.
- **Dashboard:** visualizes pass gates, failure modes, metric means, and top findings.
- **Evidence pack:** Markdown plus zipped artifacts for portable review.
- **What I measure:**
  - Policy adherence.
  - Query blast radius.
  - Lineage coverage.
  - Iceberg-context fit.

## Build Acceptance Criteria

- Deterministic local fixtures.
- Domain-specific metrics and failure modes.
- Passing unit tests.
- Passing CLI verifier.
- Static dashboard generated locally.
- Benchmark output under the project `outputs/` folder.
- Public-safe README: no founder emails, no private outreach text, no credentials.
