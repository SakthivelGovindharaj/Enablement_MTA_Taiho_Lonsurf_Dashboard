<!-- managed by Enablement_MTA_Agent -->
# LONSURF HCP Omnichannel MTA Dashboard

HCP omnichannel multi-touch attribution dashboard for **TAIHO LONSURF**, built and maintained by the
Enablement_MTA_Agent from the approved prototype `SakthivelGovindharaj/Enablement_MTA_Generic_Dashboard_Template`.

| | |
|---|---|
| Dashboard | [`index.html`](index.html) - self-contained HTML |
| Data source | Snowflake `SAND_INTOUCH_RAW_DB."AD-HOC".OMNICHANNEL_MTA_TAIHO_LONSURF` |
| Channels | Display, ENL, P2P, Email, EHR, Custom |
| Data covered | 2025-01-01 to 2026-09-01 |
| Last build | `20260929-165448` |

## Files

- `index.html` - the published dashboard (data, insights and branding injected into the approved template)
- `template/prototype.html` - this brand's approved template (layout changes only with explicit approval)
- `CHANGELOG.md` - every build, refresh, ad-hoc change and publish, with rationale
- `assets/` - brand assets (logo)
- `METHODOLOGY.md` - how every volume, KPI, attribution model, validation check and insight is produced
- `sql/` - the exact Snowflake queries the last build ran
- `.mta/` - non-secret project config and build/deploy/test logs, so the project can be resumed from this repo
