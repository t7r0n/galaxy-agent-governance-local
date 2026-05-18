# Research And Plan Review

Company: Starburst Data
Project: `galaxy-agent-governance`

## Refined Thesis

Agentic SQL over federated data needs proof that governed data products constrain discovery, access, and generated queries.

The implementation is intentionally local and synthetic, but the test harness is shaped around the real operating question: can the proposed artifact create evidence a founder, CTO, or product leader would immediately recognize as useful?

## Fresh Sources Checked

- https://www.starburst.io/blog/starburst-galaxy-ai-on-iceberg-foundations/
- https://www.starburst.io/blog/starburst-galaxy-infrastructure-planes/
- https://docs.starburst.io/

## Plan Excerpt Used

## The Gap

Starburst's October 2025 announcement promised an MCP server, a multi-agent API, and vector-store interoperability across Iceberg / PGVector / Elasticsearch — *all on top of Trino's existing row/column governance*. The public OSS surface does not reflect this. There is no `starburstdata/trino-mcp` or `starburstdata/agent-workflows` repo a customer can run, no `dbt-trino`-style ecosystem hook for the agent layer, and no benchmark that shows the *governance* property Borgman keeps citing as the differentiator. Meanwhile Databricks ships an MCP server publicly and Snowflake ships Cortex. Starburst's most defensible claim — *open standards, query in place, fine-grained governance baked into the query engine, vector store of your choice* — has no public artifact a Coatue analyst or a Citi MD can point to as proof that the agent layer respects those properties on real data. This is the gap: **a public, runnable, governance-first agentic RAG harness that demonstrably composes Trino + Iceberg + a vector store of the customer's choice, with row-level masking visible in every agent action.**

## The Project — `trino-agent-bench`

> An open, reproducible benchmark + reference agent stack that proves Starburst's "Agentic Workforce" actually honors row-level masking, column-level access, and data sovereignty — across Iceberg, PGVector, and Elasticsearch — on a 1 TB regulated-data corpus.

1. **What it is.** A docker-compose + Helm chart that brings up Trino (Starburst Galaxy-compatible), an Iceberg lakehouse on MinIO, a PGVector instance, and an Elasticsearch node, all wired through an MCP server that exposes Trino as a first-class agent tool. On top, a reference agent (`trino-agent`) executes a benchmark suite of 50 RAG queries from a synthetic-but-realistic banking corpus (KYC narratives, transaction histories, regulatory filings), with a *governance harness* that simulates four user roles (analyst, MD, auditor, public). The benchmark measures three things simultaneously: **answer quality** (LLM-judge + golden answers), **governance correctness** (zero leakage of masked columns into LLM context — verified by canary tokens), and **latency** (p50/p95 end-to-end).
2. **Why it solves the gap.** It turns Borgman's three favorite talking points — *"data access wins"*, *"row level, column level, masking"*, and *"open access to vector stores without lock-in"* — into a number a board director can quote. Coatue's analyst gets a head-to-head comparison table against Databricks (Unity Catalog masking + Mosaic + vector search) and Snowflake (Cortex + masking policies + vector). The benchmark is *Starburst-favorable by construction*, because the governance-as-default architecture is what the harness measures.
3. **The "wow" moment.** A 90-second demo: an "auditor" role asks the agent *"summarize this customer's KYC narrative"* — gets the full narrative. The "public" role asks the same question — gets a redacted summary, *and* the harness shows a green check on "zero masked-column leakage" verified by canary tokens hidden in the source data. The same query is run against an equivalent Databricks Unity Catalog setup; the canary token leaks. One slide, one number, one moat — exactly the artifact a Coatue partner can forward.

## Prototype Plan (the shippable demo)

- **The exact CLI / web surface:**
  ```bash
  # bring up the stack
  docker compose --profile bench up -d
  # load the salted corpus
  trino-agent-bench seed --rows 1_000_000 --canaries
  # run the suite for all three vector stores × four roles
  trino-agent-bench run --vectors iceberg,pgvector,es --roles analyst,md,auditor,public
  # open the comparison report
  trino-agent-bench report --vs databricks,snowflake --out report.html
  ```
- **5 demo flows / inputs:**
  1. **Auditor view of a KYC narrative** — full access, no canary hits.
  2. **Public view of the same record** — masked columns, the LLM never sees them; canary check passes.
  3. **MD view across customers** — row-level filter to MD's book, latency reported.
  4. **Comparative Databricks Unity Catalog run** — using the same corpus + roles via Databricks SQL + a Unity-Catalog-aware agent (Mosaic), surfaces the leakage delta if any.
  5. **Vector-store cost-quality plot** — Iceberg vs PGVector vs ES on the same 50 questions.
- **What we measure:**
  - **Answer quality:** LLM-judge agreement with golden answers ≥ 0.85.
  - **Governance correctness:** **zero** canary hits across 50 questions × 4 roles × 3 vector stores = 600 trials.
  - **Latency:** p95 < 4.5s on a single-node Trino + a 1 TB corpus.
  - **Reproducibility:** entire benchmark runnable on a 2024-era MacBook Pro under 30 minutes.


## Build Acceptance Criteria

- Deterministic local fixtures.
- Domain-specific metrics and failure modes.
- Passing unit tests.
- Passing CLI verifier.
- Static dashboard generated locally.
- Benchmark output under the project `outputs/` folder.
- Public-safe README: no founder emails, no private outreach text, no credentials.
