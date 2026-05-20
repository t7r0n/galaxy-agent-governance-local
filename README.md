# Trino Policy Receipts Local

Agentic SQL over federated data needs proof that governed data products constrain discovery, access, and generated queries.

The demo hook is concrete: A red/yellow/green map shows exactly which generated Trino queries violate data-product boundaries.

## Intent

Offline governance and blast-radius harness for Trino-backed AI datan agents.

## What the code proves

- Seeds `data product` fixtures for `trino-policy-receipts` with both normal operations and faulted paths.
- Computes `policy_adherence`, `query_blast_radius`, `lineage_coverage`, and `iceberg_context_fit` from deterministic inputs so the result can be reproduced exactly.
- Stress-tests `cross_catalog_leak`, `ungoverned_join`, `pii_column_touch`, and `semantic_scope_drift` as named failure classes rather than vague edge cases.
- Packages `Trino Policy Receipts Local` artifacts for code review, live demo, and regression comparison.

## Local run

```bash
uv sync --extra dev
uv run trino-policy-receipts init-demo --force
uv run trino-policy-receipts run-suite
uv run trino-policy-receipts verify
uv run trino-policy-receipts dashboard
uv run trino-policy-receipts benchmark --iterations 100
uv run trino-policy-receipts export-demo-pack
```

## Produced files

- `data/scenarios.json`
- `outputs/summary.json`
- `outputs/reports.json`
- `outputs/evidence_pack.md`
- `outputs/dashboard.html`
- `outputs/benchmark.json`
- `outputs/demo-pack.zip`

## Gatekeeping

```bash
uv run ruff check .
uv run pytest -q
uv run trino-policy-receipts run-suite
uv run trino-policy-receipts verify
uv run trino-policy-receipts benchmark --iterations 100
```

## Operational boundary

Every example in `trino-policy-receipts-local` is fabricated for repeatability. Generated outputs are rebuildable artifacts, not source material.
