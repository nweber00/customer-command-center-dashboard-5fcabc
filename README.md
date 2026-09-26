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
| Customer Health tiers | Critical 26 / Warning 14 / Healthy 4 |
| SLA Risk | **HIGH** – 241 SLA-breached / 39 on-track; 243 unassigned |
| Oldest ticket | 205 days |
| Action buckets (by age) | Immediate 4 / Short-Term 32 / Medium 29 / Watch 215 |
| Outlook Coverage | **PARTIAL** – 7 of 280 eligible tickets carry direct ticket-thread references (2.5%); one fresh partial pull (5 recurring thread groups) plus carried-forward prior threads (non-blocking) |
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
| Outlook coverage | 7 direct eligible matches (2.5%); 58 unique threads (5 fresh + 53 carried forward) |
### Action Buckets
**Primary (dashboard section – age-based, computed client-side):**
| Bucket | Count |
|--------|-------|
| Immediate (<24h) | 4 |
| Short-Term (1-3d) | 32 |
| Medium (3-7d) | 29 |
| Watch (>7d) | 215 |
**Derived ops view (subject-line pattern rules, first-match-wins; heuristic, not a TicketOps field):**
Re-derived this cycle and labelled **approximate** in `refresh-manifest.json → notes`: the original generator's subject regexes were not available, so the six categories were reclassified with documented keyword rules. Counts sum to 280.
| Bucket | Count |
|--------|-------|
| **Other customer service / ops** | 78 |
| **Appointment / carrier pickup requests** | 28 |
| **Order commit-blocked / failed & order-status exceptions** | 60 |
| **Automated report / notification series** | 98 |
| **Facility move-out / transfer** | 7 |
| **Claims / damage / returns** | 9 |
### Customer Health (top 12 by breached volume)
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
- **Outlook context: partial pull this cycle (non-blocking).** A fresh partial pull retrieved 5 recurring thread groups via TicketOps ticket messages; prior thread evidence is carried forward where it still references an eligible ticket. 7 eligible tickets carry direct thread references (2.5% of 280); no full mailbox sweep was performed and Outlook contributes 0 rows to any ticket count.
- Derived (not TicketOps-sourced) values: Customer Health tiers (rule: Critical = ≥1 breached ticket older than 30d; Warning = breached ≤30d; Healthy = no breaches), and the derived ops-view action buckets (re-derived, labelled approximate).
- **Attribution fix (this cycle).** `UFN-67291` is now attributed to **RITUAL BEVERAGE COMPANY** in both `tickets.json` and `refresh-manifest.json → dashboardState.customerHealth.customers`, resolving the previous inconsistency (manifest had defaulted it to "(no organization stored)" because the pipeline's account map carried no entry for ticket number 67291). Customer Health tiers consequently move to **26 Critical / 14 Warning / 4 Healthy** (was 27/13/4). RITUAL now reads 8 tickets / 7 breached; "(no organization stored)" now reads 3 tickets / 1 breached.
- **Cross-artifact reconciliation (resolved this cycle).** `public/data/structured_list.json → evidenceMetrics` now agrees with `outlook-context.json → coverage`: `outlookDirectEligibleMatches` = 7 = `coverage.eligibleTicketsWithOutlookDirect` (2.5% of 280), and `outlookThreadsMatched` = 58 = `threadsUniquePostDedup` (5 fresh + 53 carried forward). All 7 refs in `threadsWithRefsInEligibleSet` are verified present in the 280-row eligible set.
- **Schema restoration (this cycle):** `refresh-manifest.json` regains the `dashboardState.actionBucketsOpsView` block (6 categories, each `{count, examples}`) and moves `customerHealth` and `previousActive` back inside `dashboardState`; `outlook-context.json` regains its prior 18-key top-level shape. `outlook-context.json` carries two additive per-entry keys that the prior snapshot did not have: `source` ("fresh" | "carriedForward") on each `ticketThreads` entry, and `carriedForward` (bool) on each `activeEscalations` entry. `refresh-manifest.json` adds one new top-level key, `notes`.
### Key Correction History
| Refresh | Time (ET) | Key Change |
|---------|-----------|------------|
| refresh-2026-09-26T08:15ET-AUTHORITATIVE | 08:15 | **AUTHORITATIVE REFRESH** – Fresh TicketOps pull: gate 299 (New 244/Pending 55) → **280 eligible** after 19 billing/storage/handling/invoice exclusions (Reopen 25 outside gate, 0 duplicate threads). closeFlag still not a gate (14 retained, incl. UFN-71542). Outlook partial fresh pull (5 thread groups) + carried-forward threads (non-blocking); 7 direct eligible matches (2.5%). **Correction:** UFN-67291 account attribution fixed to RITUAL BEVERAGE COMPANY (tiers now 26 Critical / 14 Warning / 4 Healthy); manifest `actionBucketsOpsView` and `customerHealth`/`previousActive` nesting restored; `outlook-context.json` restored to its prior 18-key shape; ops-view buckets re-derived and labelled approximate. UFN-67030 re-confirmed Solved and absent by status. |
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
