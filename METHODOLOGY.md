<!-- managed by Enablement_MTA_Agent - regenerated on every publish -->
# Methodology - LONSURF MTA dashboard

Build `20260929-165448` · source `"SAND_INTOUCH_RAW_DB"."AD-HOC"."OMNICHANNEL_MTA_TAIHO_LONSURF"` · data 2025-01-01 to 2026-09-01.
The exact SQL for this build is in [`sql/`](sql/).

## 1. Source and column mapping

Media (touchpoint) rows: `"DATA_SOURCE" IN ('PLD', 'TARGET_ONLY')` · prescription rows: `"DATA_SOURCE" IN ('PRESCRIPTION',)`.

| Dashboard field | Source column |
|---|---|
| `npi` | `NPI` |
| `date` | `DATE` |
| `channel` | `CHANNEL` |
| `partner` | `PARTNER` |
| `reached_flag` | `IS_HCP_REACHED` |
| `engaged_flag` | `IS_HCP_ENGAGED` |
| Script slot `trx` (shown as **TRx**) | `TRX` |
| Script slot `nbrx` (shown as **NBRx**) | `NBRX` |
| Script slot `nrx` (shown as **NRx**) | `NRX` |
| Spend | not available |
| HCP attributes (filters) | `{'table': 'OMNICHANNEL_MTA_TAIHO_LONSURF', 'npi': 'NPI', 'specialty': 'HCP_SPECIALTY', 'affiliation': None, 'stage': 'HCP_CURRENT_JOURNEY_STAGE'}` |
| Control group | `unexposed_prescribers` |

All identifiers are double-quoted in SQL (schemas such as `AD-HOC` and columns with spaces/mixed case require it).
Media rows with a null HCP, date or channel are excluded (counted by the validation checks).

## 2. Volumes and KPIs

| KPI | Definition | SQL |
|---|---|---|
| Targeted HCPs | Distinct HCPs with any media touch (no separate target list) | `touches.sql` |
| HCPs reached | Targeted HCPs flagged reached (`IS_HCP_REACHED`) | `touches.sql` |
| HCPs engaged | HCPs with any engaged touch (`IS_HCP_ENGAGED`) | `touches.sql` |
| Engaged HCPs w/ scripts | Engaged HCPs with attributable scripts | `touches.sql` + `rx.sql` |
| Engaged HCP total scripts | Attributable scripts of those HCPs, per selected metric | `touches.sql` + `rx.sql` |
| Avg. time engagement → script | Median days from an HCP's first engaged touch to their first script on/after it | `touches.sql` + `rx.sql` |
| Monthly sparklines / deltas | Distinct reached / engaged HCPs per month; deltas compare the selected period with the prior period of equal length | `media_month.sql` |

**Attributable scripts:** a script counts only if it falls in or after the month of the HCP's first media touch (pre-exposure scripts are not credited).

## 3. Attribution models

| Model | Rule |
|---|---|
| Last touch | All of an HCP's attributable scripts go to the channel most recently active on/before their first attributable script (no scripts: the channel touched last) |
| First touch | All go to the channel with the HCP's earliest touch |
| Multi-touch (linear) | Split equally across the distinct channels on the HCP's path |
| Channel lift | Exposed vs control script rate per channel; channel incremental scripts are normalised to the overall incremental total |
| MTA (Markov) | Removal effect on the conversion probability of the channel-transition graph (paths ordered by first touch per channel); credit = total last-touch scripts x normalised removal effect (1% floor for zero-effect channels) |
| MTA (Shapley) | Average marginal contribution of each channel across all channel coalitions of converting HCPs; credit = total scripts x normalised Shapley value |

The partner table's "Incremental Scripts" column is NPI-level last-touch TRx per partner x channel.
Incrementality: control group mode `unexposed_prescribers`.

## 4. Validation (runs before anything is published)

| Check | Fails / warns when |
|---|---|
| row_count | fewer than 1,000 rows (fail); >20% drop vs last good run (warn) |
| nulls_* | null HCP / date / channel above {'npi': 0.5, 'date': 1.0, 'channel': 1.0} % of media rows (warn) |
| freshness_media / _rx | latest media older than 14 days / prescriptions older than 45 days (warn) |
| rx_to_media_npi_match | fewer than 1 prescribers match an exposed HCP (fail) |
| attributable_scripts | no attributable scripts (fail) |
| partial_latest_rx_month | latest month's prescribers below 0.5 x the prior-3-month median (warn) |
| layout_drift | page structure differs from the approved template (fail, never waivable) |

Fail → nothing is published and a failure email is sent. Warn → a pull request is opened for review, not merged.

## 5. Insights

Every build and daily refresh rewrites all Key Observation texts from scratch: attribution (one per model), trend,
incrementality (channel and exposed-vs-control), partner table and the email summary. They are written by Claude Sonnet 5
(Vertex AI) following `mta_agent/skills/insight_writing/SKILL.md`:
- **Analysis, not a number swap:** they compare against the prior period, the previous build, other models, the funnel and
  efficiency.
- **Grounded:** every number must exist in this build's facts, which is checked automatically; an ungrounded section falls
  back to fixed text and flags the build.
- **Sections without data** show a standard *Data not available* note instead of text.

The page re-applies these insights after its own scripts run, including when a different attribution model is selected.
Everything else on the page is computed by code, not by the model.

## 6. Visual alignment

After injection, every build is rendered in headless Chrome next to the approved template:
- **Measured check (can fail the build):** component counts, containment, left edges and widths, overflow and clipped content.
- **Visual review (advisory):** Claude compares the two screenshots.

A major measured difference fails the build, so it is never published.
