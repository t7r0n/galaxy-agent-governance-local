# Galaxy Agent Governance Local

Offline governance and blast-radius harness for Trino-backed AI data agents.

This is a local-first, synthetic-data prototype inspired by a company-specific project plan for **Starburst Data**. It is built to demonstrate the engineering shape of `galaxy-agent-governance` without private data, credentials, external APIs, or hosted services.

## Why it matters

Agentic SQL over federated data needs proof that governed data products constrain discovery, access, and generated queries.

## What it does

- Generates deterministic synthetic `data product` scenarios.
- Scores each scenario against domain-specific quality gates.
- Produces evidence-backed findings for realistic failure modes.
- Writes a static dashboard, JSON reports, benchmark output, and a portable demo pack.
- Exposes a JSONL tool loop for local agent integration.

## Metrics

- `policy_adherence`
- `query_blast_radius`
- `lineage_coverage`
- `iceberg_context_fit`

## Failure modes

- `cross_catalog_leak`
- `ungoverned_join`
- `pii_column_touch`
- `semantic_scope_drift`

## Quickstart

```bash
uv sync --extra dev
uv run galaxy-governance init-demo --force
uv run galaxy-governance run-suite
uv run galaxy-governance verify
uv run galaxy-governance dashboard
uv run galaxy-governance benchmark --iterations 100
uv run galaxy-governance export-demo-pack
```

## Expected outputs

- `data/scenarios.json`
- `outputs/summary.json`
- `outputs/reports.json`
- `outputs/evidence_pack.md`
- `outputs/dashboard.html`
- `outputs/benchmark.json`
- `outputs/demo-pack.zip`

## Validation

```bash
uv run ruff check .
uv run pytest -q
uv run galaxy-governance run-suite
uv run galaxy-governance verify
uv run galaxy-governance benchmark --iterations 100
```

## Demo hook

A red/yellow/green map shows exactly which generated Trino queries violate data-product boundaries.
