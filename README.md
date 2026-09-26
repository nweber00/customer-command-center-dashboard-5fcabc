# Customer Command Center Dashboard – NHT/Cesanek
**Facility**: NHT/Cesanek (LT_F21) | **Tenant**: LT | **Timezone**: America/New_York
Deployed at: [customer-command-center-dashboard-5fcabc.coolify.item.pub/dashboard](https://customer-command-center-dashboard-5fcabc.coolify.item.pub/dashboard)
## Overview
Real-time operational dashboard for monitoring ticket queues, customer health, and action priorities at the NHT/Cesanek facility.
## Ticket Filtering Rules
| Rule | Implementation |
|------|----------------|
| **Include Statuses** | New, Open, Pending |
| **Exclude Statuses** | Reopen, Reopened, Closed, Resolved, Cancelled, Done |
| **Exclude Invoice Items** | billing, UF Billing, storage, handling |
| **UFN Filtering** | Enabled – UFN-tagged tickets surfaced in all views |
| **Deduplication** | Overlapping ticket/email threads merged |
| **Outlook Context** | Non-blocking enrichment where available |
| **Authority** | TicketOps status is authoritative |
| **Eligibility Gate** | `displayStatusSystemStatus = open` **AND** `displayStatusName ∈ {New, Pending}` |
| **closeFlag Gate** | **NOT USED** – closeFlag is not a filter; live `closeFlag=true` tickets are retained (auto-close artifacts cause false negatives) |
## Customer Health
Coverage rule: All customers visible in eligible NHT/Cesanek tickets. Configured roster/aliases are supplemental only.
## Dashboard Sections
1. **Counts** – Total eligible, by status, UFN-tagged, excluded
2. **Priority Queue** – Sorted by priority & age
3. **Action Buckets** – Immediate (<24h), Short-Term (1–3d), Medium (3–7d), Watch (>7d)
4. **Customer Health** – Per-customer ticket counts, aging, UFN exposure, health ratings
5. **Evidence & Metrics** – Outlook matches, dedup stats, invoice exclusions, SLA risk, freshness
## Current Dashboard State (Last Refresh: Sep 26 2026 08:15 ET – AUTHORITATIVE)
| Metric | Value |
|--------|-------|
| Department total (all statuses) | not re-pulled this cycle (last known 71,359 from 2026-09-24) |
| Open system-status rows | 324 = New 244 + Pending 55 + Reopen 25 |
| Gate matched | **299** (New 244, Pending 55, Open 0) |
| Eligible | **280** (230 New, 0 Open, 50 Pending) |
| Excluded | 19 billing/UF Billing/storage/handling/invoice items + 0 overlapping same-issue duplicates; Reopen 25 outside gate |
| closeFlag | **NOT a gate** — 14 live `closeFlag=true` rows retained (incl. UFN-71542) |
| Customers | **44** distinct values (all customers visible in eligible tickets; roster supplemental) |
| Customer Health tiers | Critical 27 / Warning 13 / Healthy 4 |
| SLA Risk | **HIGH** – 241 SLA-breached / 39 on-track; 243 unassigned |
| Oldest ticket | 205 days |
| Action buckets (by age) | Immediate 4 / Short-Term 32 / Medium 29 / Watch 215 |
| Outlook Coverage | 5 email-thread groups pulled this cycle (non-blocking); 5 direct eligible matches |
| Last Refresh | 2026-09-26 08:15 ET (**AUTHORITATIVE** – fresh TicketOps pull, 299 gate rows) |
| Previous refresh | 2026-09-24 06:40 ET — 278 eligible |
| Next Refresh | ~08:00 ET (daily summary email) |
> **Eligibility gate applied:** `displayStatusSystemStatus = open` **AND** `displayStatusName ∈ {New, Pending}`. `closeFlag` is **not** a gate. Note there is **no "Open" display status** in this department — the third open-system-status state is **Reopen (25)**, which the rules exclude.
> **Premise note (recurring):** the instruction again cited UFN-67030 as "live-Pending with closeFlag=true". Ticket Ops is authoritative and shows UFN-67030 = `displayStatusName=Solved`, `displayStatusSystemStatus=20`, staff-closed 2026-09-01. It is excluded **by status**, not by `closeFlag`, and is correctly absent from the 280 eligible rows. The rule stands; the rows that actually exercise it are the 14 retained `closeFlag=true` rows.
### Evidence Metrics
| Metric | Value |
|--------|-------|
| Total eligible | 280 |
| SLA breached / on-track | 241 / 39 |
| Unassigned | 243 |
| Aging 15d+ | 188 |
| Billing items excluded | 19 |
| Reopen excluded | 25 |
| Overlap duplicates removed | 0 |
| closeFlag=true retained | 14 |
| Oldest ticket age | 205d |
### Action Buckets
**Primary (dashboard section – age-based, computed client-side):**
| Bucket | Count |
|--------|-------|
| Immediate (<24h) | 4 |
| Short-Term (1-3d) | 32 |
| Medium (3-7d) | 29 |
| Watch (>7d) | 215 |
**Derived ops view (subject-line pattern rules, first-match-wins; heuristic, not a TicketOps field):**
Not recomputed for this snapshot. The pattern rules are not carried in the refresh output for 2026-09-26, and the prior counts (103 / 95 / 28 / 23 / 15 / 14) were derived against the 278-row 2026-09-24 set. A best-effort keyword replay did not reproduce those counts, so no new grouping is published here rather than publishing an unverified one. Prior values remain in git history.
### Customer Health (top 12 by breached volume)
Derived from the row-level `tickets.json` artifact (what the dashboard renders). See the attribution note under Data Freshness Gaps for one row where `refresh-manifest.json` differs.
| Customer | Tickets | Breached | Oldest breached | Tier |
|----------|---------|----------|-----------------|------|
| LASSONDE PAPPAS AND COMPANY, INC. | 68 | 65 | 86d | Critical |
| TURTLE BEACH | 20 | 20 | 205d | Critical |
| MIDEA AMERICA CORP | 20 | 19 | 57d | Critical |
| DAYDREAM NUTRITION INC. | 19 | 19 | 43d | Critical |
| PRIME TIME PACKAGING LTD | 17 | 16 | 173d | Critical |
| NIAGARA BOTTLING LLC | 13 | 13 | 183d | Critical |
| TCL NORTH AMERICA | 16 | 12 | 10d | Warning |
| CANVAS 340 LLC | 12 | 12 | 75d | Critical |
| SMEG USA INC | 10 | 10 | 197d | Critical |
| RITUAL BEVERAGE COMPANY | 8 | 7 | 179d | Critical |
| HINT INC. | 10 | 6 | 30d | Warning |
| Natural Rapport (Q & C Products LLC) | 4 | 4 | 68d | Critical |
### Priority Queue (top 10 by age)
| Rank | Ticket | Customer | Subject | Age | SLA | Owner |
|------|--------|----------|---------|-----|-----|-------|
| 1 | UFN-33604 | TURTLE BEACH | Pending/Overdue Tickets | 205d / 4928h | Breached | unassigned |
| 2 | UFN-33722 | ZEN BEVERAGE | 12x9x12 Boxes Needed | 205d / 4928h | Breached | unassigned |
| 3 | UFN-35588 | ATERIAN INC | Appointment Request :: DN-1467998 RN-23177 ATERIAN, INC. | 198d / 4760h | Breached | unassigned |
| 4 | UFN-35774 | SMEG USA INC | Commit Block Order - DN-1467983 | 197d / 4736h | Breached | Sittie Jhaila Sirad |
| 5 | UFN-37858 | WYNK BEVERAGE - Reverse | welcome, spring | 189d / 4544h | Breached | unassigned |
| 6 | UFN-39065 | TURTLE BEACH | Reminder: SEND VIVO DAMAGE REPORT | 185d / 4448h | Breached | unassigned |
| 7 | UFN-39403 | SMEG USA INC | Reminder following up! | 184d / 4424h | Breached | unassigned |
| 8 | UFN-39662 | NIAGARA BOTTLING LLC | NIAGARA//JEFF-0327-SRICHAKRA//JEFFERSONVILLE IN | 183d / 4400h | Breached | unassigned |
| 9 | UFN-40213 | NIAGARA BOTTLING LLC | Re: QUANTIX SCHEDULE - 3/28 & 3/30 | 180d / 4328h | Breached | unassigned |
| 10 | UFN-40654 | NIAGARA BOTTLING LLC | NIAGARA/PLAINFIELD/ DUYTAN/ 4.6- 4.12 | 179d / 4304h | Breached | unassigned |
### Data Freshness Gaps
- `lastUpdated` and `assigned` were supplied by Ticket Ops for all 280 rows; `assigned` empty means unassigned at source (243 of 280).
- **Outlook context: partial pull this cycle (non-blocking).** 5 representative email-thread groups were retrieved via TicketOps ticket messages; this is a top-thread sample, not a full mailbox sweep, and it contributes 0 rows to any ticket count.
- Derived (not TicketOps-sourced) values: Customer Health tiers (rule: Critical = ≥1 breached ticket older than 30d; Warning = breached ≤30d; Healthy = no breaches). The ops-view action buckets are **not** recomputed this cycle.
- **Attribution note (one row):** `refresh-manifest.json → customerHealth.customers` attributes UFN-67291 to "(no organization stored)" (giving RITUAL 7 / blank 4), whereas the row-level `tickets.json` labels that row **RITUAL BEVERAGE COMPANY** (RITUAL 8 / blank 3). Cause: the refresh pipeline's account map has no entry for ticket number 67291, so it defaults to "(no organization stored)". Headline totals reconcile either way (280 tickets, 241 breached, 44 customers), but the tier split does not: the manifest's 27 Critical / 13 Warning reflects the default attribution, while row-level attribution yields **26 Critical / 14 Warning** — "(no organization stored)" drops to Warning (its only remaining breached row is 8d old) and RITUAL stays Critical. The state table above publishes the snapshot's values; the Customer Health table below uses the row-level artifact.
### Key Correction History
| Refresh | Time (ET) | Key Change |
|---------|-----------|------------|
| refresh-2026-09-26T08:15ET-AUTHORITATIVE | 08:15 | **AUTHORITATIVE REFRESH** – Fresh TicketOps pull: gate 299 (New 244/Pending 55) → **280 eligible** after 19 billing/storage/handling/invoice exclusions (Reopen 25 outside gate, 0 duplicate threads). closeFlag still not a gate (14 retained, incl. UFN-71542). Outlook context partial (5 thread groups, non-blocking). UFN-67030 re-confirmed Solved and absent by status. |
| refresh-2026-09-24T06:40ET-AUTHORITATIVE | 06:40 | **AUTHORITATIVE REFRESH** – Fresh TicketOps pull: gate 295 (New 237/Pending 58) → **278 eligible** after 17 billing/storage/handling/invoice exclusions (Reopen 32 outside gate, 0 overlap duplicates). closeFlag still not a gate (14 retained). Outlook pull unavailable (non-blocking). UFN-67030 re-confirmed Solved. |
| refresh-2026-09-22T04:17ET-AUTHORITATIVE | 04:17 | Fresh TicketOps pull: gate 297 → 268 eligible after 21 billing and 8 overlap duplicates; 23 closeFlag=true retained. |
| refresh-2026-09-15T23:45ET-AUTHORITATIVE | 23:45 | Gate 312 → 291 eligible. 16 billing + 5 duplicates excluded. |
| refresh-2026-09-15T12:36ET-AUTHORITATIVE | 12:36 | Gate 312 → 286 eligible — superseded same day. |
## Data Files
- `config.json` – Dashboard configuration & filter rules
- `dashboard/data/tickets.json` – Current eligible ticket data (TicketOps source)
- `dashboard/data/outlook-context.json` – Outlook email thread context (non-blocking)
- `dashboard/data/refresh-manifest.json` – Complete refresh audit with rules applied and evidence metrics
- `public/data/tickets.json` – Public-facing tickets (synced)
- `public/data/structured_list.json` – Public-facing structured dashboard data
- `public/data/outlook-context.json` – Public-facing Outlook context (synced)
- `public/data/refresh-manifest.json` – Public-facing refresh summary
## Repository
- **Owner**: nweber00
- **Repo**: customer-command-center-dashboard-5fcabc
- **URL**: https://github.com/nweber00/customer-command-center-dashboard-5fcabc
