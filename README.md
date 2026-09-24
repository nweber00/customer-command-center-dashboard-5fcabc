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
## Current Dashboard State (Last Refresh: Sep 24 2026 06:40 ET – AUTHORITATIVE)
| Metric | Value |
|--------|-------|
| Department total (all statuses) | 71,359 |
| Open system-status rows | 327 = New 237 + Pending 58 + Reopen 32 |
| Gate matched | **295** (New 237, Pending 58) |
| Eligible | **278** (223 New, 0 Open, 55 Pending) |
| Excluded | 17 billing/UF Billing/storage/handling/invoice items + 0 overlapping same-issue duplicates; Reopen 32 outside gate |
| closeFlag | **NOT a gate** — 14 live `closeFlag=true` rows retained |
| Customers | **50** distinct values incl. contact-only rows (all customers visible in eligible tickets; roster supplemental) |
| Customer Health tiers | Critical 27 / Warning 15 / Healthy 8 |
| SLA Risk | **HIGH** – 235 SLA-breached / 43 on-track; 240 unassigned |
| Oldest ticket | 202 days |
| Action buckets (by age) | Immediate 28 / Short-Term 20 / Medium 17 / Watch 213 |
| Outlook Coverage | **UNAVAILABLE** – no fresh Outlook pull this cycle (non-blocking); 0 threads merged |
| Last Refresh | 2026-09-24 06:40 ET (**AUTHORITATIVE** – fresh TicketOps pull, 295/295 rows enumerated) |
| Previous refresh | 2026-09-22 04:17 ET — 268 eligible |
| Next Refresh | ~08:00 ET (daily summary email) |
> **Eligibility gate applied:** `displayStatusSystemStatus = open` **AND** `displayStatusName ∈ {New, Pending}`. `closeFlag` is **not** a gate. Note there is **no "Open" display status** in this department — the third open-system-status state is **Reopen (32)**, which the rules exclude.
> **Premise note (recurring):** the instruction again cited UFN-67030 as "live-Pending with closeFlag=true". Ticket Ops is authoritative and shows UFN-67030 = `displayStatusName=Solved`, `displayStatusSystemStatus=20`, staff-closed 2026-09-01. It is excluded **by status**, not by `closeFlag`. The rule stands.
### Evidence Metrics
| Metric | Value |
|--------|-------|
| Total eligible | 278 |
| SLA breached / on-track | 235 / 43 |
| Unassigned | 240 |
| Aging 15d+ | 176 |
| Billing items excluded | 17 |
| Reopen excluded | 32 |
| Overlap duplicates removed | 0 |
| closeFlag=true retained | 14 |
| Oldest ticket age | 202d |
### Action Buckets
**Primary (dashboard section – age-based, computed client-side):**
| Bucket | Count |
|--------|-------|
| Immediate (<24h) | 28 |
| Short-Term (1-3d) | 20 |
| Medium (3-7d) | 17 |
| Watch (>7d) | 213 |
**Derived ops view (subject-line pattern rules, first-match-wins; heuristic, not a TicketOps field):**
| Bucket | Count | Examples |
|--------|-------|----------|
| **Automated report / notification series** | 103 | UFN-39065, UFN-40670, UFN-40860, UFN-42645, UFN-44321, UFN-45763 |
| **Other customer service / ops** | 95 | UFN-33604, UFN-33722, UFN-37858, UFN-39403, UFN-39662, UFN-40213 |
| **Appointment / carrier pickup requests** | 28 | UFN-35588, UFN-40969, UFN-46639, UFN-47786, UFN-51184, UFN-54684 |
| **Order commit-blocked / failed & order-status exceptions** | 23 | UFN-35774, UFN-67835, UFN-68017, UFN-68029, UFN-68573, UFN-69380 |
| **Facility move-out / transfer** | 15 | UFN-43887, UFN-64739, UFN-70161, UFN-70572, UFN-70573, UFN-70732 |
| **Claims / damage / returns** | 14 | UFN-46564, UFN-56957, UFN-59238, UFN-59296, UFN-59777, UFN-61390 |
### Customer Health (top 12 by breached volume)
| Customer | Tickets | Breached | Oldest breached | Tier |
|----------|---------|----------|-----------------|------|
| LASSONDE PAPPAS AND COMPANY, INC. | 66 | 63 | 83d | Critical |
| Turtle Beach | 20 | 20 | 202d | Critical |
| Midea America Corp | 20 | 19 | 54d | Critical |
| DAYDREAM NUTRITION INC. | 19 | 19 | 41d | Critical |
| PRIME TIME PACKAGING LTD | 16 | 16 | 171d | Critical |
| NIAGARA BOTTLING LLC | 13 | 13 | 180d | Critical |
| ATERIAN GROUP, INC. | 11 | 9 | 23d | Warning |
| CANVAS 340 LLC | 12 | 8 | 72d | Critical |
| SMEG USA INC | 8 | 8 | 194d | Critical |
| RITUAL BEVERAGE COMPANY | 7 | 7 | 176d | Critical |
| HINT INC. | 7 | 6 | 27d | Warning |
| Natural Rapport (Q & C Products LLC) | 7 | 5 | 65d | Critical |
### Priority Queue (top 10 by age)
| Rank | Ticket | Customer | Subject | Age | SLA | Owner |
|------|--------|----------|---------|-----|-----|-------|
| 1 | UFN-33604 | Turtle Beach | Pending/Overdue Tickets | 203d / 4865h | Breached | unassigned |
| 2 | UFN-33722 | ZEN BEVERAGE | 12x9x12 Boxes Needed | 202d / 4858h | Breached | unassigned |
| 3 | UFN-35588 | ATERIAN INC | Appointment Request :: PO: DN-1467998 RN-23177 Flock Ref#: V | 195d / 4690h | Breached | unassigned |
| 4 | UFN-35774 | SMEG USA INC | Commit Block Order - DN-1467983 | 195d / 4670h | Breached | unassigned |
| 5 | UFN-37858 | WYNK BEVERAGE - Reverse | welcome, spring 🌷🍋🪻🍓 | 187d / 4480h | Breached | unassigned |
| 6 | UFN-39065 | Turtle Beach | Reminder: SEND VIVO DAMAGE REPORT | 183d / 4385h | Breached | unassigned |
| 7 | UFN-39403 | SMEG USA INC | Reminder following up! | 182d / 4358h | Breached | unassigned |
| 8 | UFN-39662 | NIAGARA BOTTLING LLC | NIAGARA//JEFF-0327-SRICHAKRA//JEFFERSONVILLE IN | 181d / 4334h | Breached | unassigned |
| 9 | UFN-40213 | NIAGARA BOTTLING LLC | Re: QUANTIX SCHEDULE - 3/28 & 3/30 | 178d / 4265h | Breached | unassigned |
| 10 | UFN-40654 | NIAGARA BOTTLING LLC | NIAGARA/PLAINFIELD/ DUYTAN/ 4.6- 4.12 | 176d / 4234h | Breached | unassigned |
### Data Freshness Gaps
- `lastUpdated` and `assigned` were supplied by Ticket Ops for all 278 rows; `assigned` empty means unassigned at source (240 of 278).
- **Outlook context is unavailable this cycle** (no mailbox tool in the refresh session). Non-blocking per the rules; `outlook-context.json` is carried forward unchanged from the 2026-09-15 window and remains labelled stale.
- Derived (not TicketOps-sourced) values: the ops-view action buckets above, and Customer Health tiers (rule: Critical = ≥1 breached ticket older than 30d; Warning = breached ≤30d; Healthy = no breaches).
### Key Correction History
| Refresh | Time (ET) | Key Change |
|---------|-----------|------------|
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
