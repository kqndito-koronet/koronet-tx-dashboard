# Price's dashboard review — Facu feedback

Status: **TRAINING EXAMPLE ONLY — NOT ACTIVE ACCOUNT RESEARCH.** Source: Facu live walkthrough of the Transactions dashboard on 2026-08-01, with dashboard snapshot and Fathom call `766563358` as context. It records semantic/data-quality feedback to improve the CoS ingestion loop. No account/data/priority change, potential calculation, visit preparation, research task or external action is authorized by this file.

## Why Price's matters

- Approx. $1.8M annual GMV in the dashboard snapshot.
- Core + Procurement + eShop is understood by Facu as the strongest product setup, with comparatively limited marketing constraints and potential to BUY and SELL.
- It is a reimplementation, not a brand-new implementation. It previously bought online via individual vendor eCommerce; Procurement has not clearly accelerated that behavior yet.
- Christine and Vale planned an in-person visit. The system should prepare a specific play, not merely a generic account brief.

## Facu's interpretation to preserve and test

### BUY

- Dashboard shows roughly 60.5% online procurement buying. Facu considers this very strong for a Core account and wants to understand what Price's likes about Procurement.
- Directional economics: $1.8M x 46% is roughly $800K addressable BUY; at ~60% online it could be roughly $500K online buying and about $7.4K in indirect fees. This is a working hypothesis; assumptions, fee rate and period must be shown before use.
- Dashboard `estimated potential` of $84K appears inconsistent with the simple directional calculation and needs lineage/definition audit.
- **Facu correction on potential modeling:** this contradiction does not mean stop estimating. Model potential as a range of explicit scenarios. The 46% purchase-share input is a benchmark derived from wholesalers on Core where Koronet can observe the full purchase-and-sale flow; it is periodically recalculable, not a universal Price's assumption. It may be used as a labelled proxy only if Price's coverage is sufficiently comparable. If it applies, $1.8M x 46% is ~ $828K directional BUY addressable; at 60.5% online, ~ $501K; at 97.5%, ~ $807K. These are not assertions of truth: they depend on Core/offline coverage, period alignment and fee-rate eligibility. The research task is to learn which scenario is closest, not to discard the account analysis.
- **Facu correction on pacing:** pair the potential range with observed pace. State the exact interval, actual purchasing per week/month, the seasonally comparable annual/period landing, variance versus each potential scenario, and the confidence/assumptions. Potential answers “what could be captured”; pace answers “where is the account heading given current behavior and the time of year.”
- Vendor/category/variety figures appear semantically broken: 26 vendors, 67 categories, 10 varieties is not plausible as displayed; “variety” may be a mislabeled field. Offline values are near zero despite ~40% non-online buying, so coverage/definition is likely incomplete.
- Required analysis: which vendors, categories, varieties/SKUs or products are still bought offline; whether they are missing connections, disconnected/leaking, or recurring vendor patterns; and whether Procurement shifted behavior relative to the historical individual-vendor eCommerce baseline.

### LIST / SELL

- The dashboard uses different category/variety measures in LIST versus BUY without clear definitions or comparable offline coverage.
- Facu expects to see what Price's sells, breadth of assortment, time horizon/depth, and the online/offline differences on comparable definitions.
- SELL currently reports six online customers while sales are shown as zero. That is a data/semantic contradiction, not a customer insight.
- Offline shows 107 customers, but comparable LIST/time-horizon evidence is absent. Dashboard must not imply a diagnosis from this incomplete mapping.
- Directional sell economics requested: 10% of $1.8M would be $180K and, at 1.5%, roughly $2.7K fees. This is illustrative until eligible GMV, applicable fee rate and channel are verified.

## Required visit play — draft research brief, not customer action

Before Christine/Vale visit, produce a concise account-specific play:

1. confirm dates/visit objective and what success looks like;
2. compare pre-reimplementation online behavior with post-Procurement behavior;
3. show BUY baseline, online share, vendor/product leakage candidates and top unanswered questions;
4. show SELL baseline, buyers, assortment breadth and time horizon with definitions/freshness;
5. identify 1–3 candidate actions that can be executed with current product, plus items needing Product/Technical review;
6. provide discovery questions for Price's and a mechanism to record answers/outcome.

## Weekly operating use

The Wednesday Cata/Christine meeting is also the recurring place to discuss how the dashboard cohort is progressing and what each account/team needs. The dashboard should therefore prepare, for Price's and each current account: change since last review, current objective, evidence/confidence, owner activity, blocker/help needed, proposed next action and the specific learning to capture. It is not only a presentation of the list.

## Dashboard remediation requirements

1. Every number must state definition, period, source and coverage. Do not use a proxy as if it were a direct measure.
2. Repair or suppress contradictory fields: online customers with zero online sales; category/variety mismatch; offline coverage gaps; unexplained $84K potential.
3. Preserve separate surfaces/channels: individual vendor eCommerce, Koronet Procurement, eShop and other online channels must never be conflated.
4. Give a per-account answer: what can it BUY, LIST and SELL now; what is missing; what evidence supports that; and what should the owner do next.
5. Add a visible data-quality state when source data is insufficient; never manufacture a lead from incomplete coverage.
6. Show directional potential as labelled scenarios/ranges when the denominator is uncertain: source metric, assumptions, sensitivity and the one query/conversation needed to collapse the range.
