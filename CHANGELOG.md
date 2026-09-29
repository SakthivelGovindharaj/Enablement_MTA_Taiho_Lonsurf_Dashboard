# Change log - LONSURF MTA dashboard

- **2026-09-29T15:17:22+05:30** [config] project created for LONSURF
- **2026-09-29T15:22:16+05:30** [config] data source set to "SAND_INTOUCH_RAW_DB"."AD-HOC"."OMNICHANNEL_MTA_TAIHO_LONSURF" (ODBC)
- **2026-09-29T15:29:15+05:30** [config] repos set: reference SakthivelGovindharaj/Enablement_MTA_Generic_Dashboard_Template, output SakthivelGovindharaj/Enablement_MTA_Taiho_Lonsurf_Dashboard
- **2026-09-29T15:37:26+05:30** [structural change] initialized brand template from reference repo @1c27c91 - _why:_ Initialize brand template for taiho_lonsurf from approved prototype so we can preview the layout before finalizing branding inputs.
- **2026-09-29T15:47:25+05:30** [config] extracted branding candidates from https://www.lonsurf.com/
- **2026-09-29T15:50:48+05:30** [config] branding updated
- **2026-09-29T15:50:48+05:30** [config] brand logo set from C:\Users\Sakthivel.Govindhara\OneDrive - EVERSANA\_Eversana_Intouch_2025_\AI Agency - Intouch\Enablement_MTA_Servier_Voranigo_Agent\projects\taiho_lonsurf\assets\candidates\logo_candidate_1.svg
- **2026-09-29T15:52:49+05:30** [config] updated notifications: recipients
- **2026-09-29T15:54:43+05:30** [config] Deferred email backend (Brevo) setup until after dashboard build; alert recipient sakthivel.govindharaj@eversana.com already saved. - _why:_ User asked to complete the dashboard build first and revisit alerts configuration later.
- **2026-09-29T16:04:49+05:30** [config] branding updated
- **2026-09-29T16:05:12+05:30** [config] column mapping saved - _why:_ RRx (RRX column) left unused - only 3 script slots available, mapped TRx=TRX, NRx=NRX, NBRx=NBRX. No spend data (MEDIA_COST 100% null). Control group set to unexposed_prescribers (auto-derived); explicit IS_CONTROL flag column left unused per user decision. HCP attributes: Specialty (HCP_SPECIALTY) and Journey Stage (HCP_CURRENT_JOURNEY_STAGE) mapped from source table; Affiliation has no data so filter shows All only. Partner display names auto-cleaned to title case.
- **2026-09-29T16:09:08+05:30** [ad-hoc fix] build 20260929-160545 (warning) covering 2025-01-01 to 2026-09-01 - _why:_ Initial dashboard build for taiho_lonsurf after data source, repos, branding, and column mapping setup
- **2026-09-29T16:52:01+05:30** [ad-hoc fix] build 20260929-164653 (warning) covering 2025-01-01 to 2026-09-01 - _why:_ verification build: layout fix + visual eval + new insights
- **2026-09-29T16:59:15+05:30** [ad-hoc fix] build 20260929-165448 (warning) covering 2025-01-01 to 2026-09-01 - _why:_ verification build 2: grounding-check and reach fixes
- **2026-09-29T17:45:47+05:30** [publish] publishing build 20260929-165448 - _why:_ Initial LONSURF MTA dashboard publish: full data load (10.02M rows, Jan 2025–Sep 2026), regenerated insights, layout verified against approved template. Known warnings: Rx data 53 days stale, Aug 2026 partial month.
