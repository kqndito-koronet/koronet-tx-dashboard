# TX external-research loop checkpoint — 2026-08-03

## Status at pause

- Research workers paused on Facu's instruction so dashboard work can take priority.
- `175` account packets are validated in `/Users/facu/Koronet_Facu_OS/tx_external_research_staging/`.
- A full idempotent staging-to-Supabase sync was started before this checkpoint.
- Durable identity key: Koronet `company_id` (`account_id` in the research tables). Never join by account name.

## Durable contract

Each packet contains source observations plus one or more estimates:

- `company_gmv` and `total_purchases` only;
- low/mid/high range, method, date, confidence and evidence links;
- `blocked` is a valid outcome only with the exact evidence/identity gap documented;
- raw source-specific labels remain in `raw_payload`; the importer maps them to the database taxonomy.

Supabase tables: `tx_research_accounts`, `tx_account_research_observations`,
`tx_account_research_estimates`, `tx_account_research_estimate_evidence`.

## Resume sequence

1. Validate all staging files with `scripts/import_tx_account_research.py` in dry mode.
2. Run `scripts/import_tx_research_batch.py /Users/facu/Koronet_Facu_OS/tx_external_research_staging --apply`.
3. Read back the remote account count and record a SHADOW receipt.
4. Compute unresearched `company_id`s from `gmv_research_queue_2026-08-03.json` minus both staging directories.
5. Run three non-overlapping research waves. Workers write staging only; importer owns Supabase writes.
6. Do not promote any estimate into dashboard metrics until the dashboard-materialization gate is built.

## Explicit boundary

This loop continues research durability. It does not alter dashboard cards, schema migrations,
plays, priorities, bridge/staging, or canonicals without a separate reviewed change.
