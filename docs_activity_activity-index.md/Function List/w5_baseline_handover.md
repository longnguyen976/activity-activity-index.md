# W5 Baseline and Handover: MySen Platform

Lesson 5 (block W5). Official baseline for release planning and execution.

## 1. Three Scenarios Summary

| Scenario | Person-hours | Cost (VND) | Role in Cash-Flow Analysis (W6) |
|---|---|---|---|
| Pessimistic: Must only | 127.6 | 35,460,000 | Minimum investment baseline |
| Base: Must + Should | 143.8 | 39,720,000 | Primary figure for client sign-off |
| Optimistic: Full scope | 160.0 | 43,980,000 | Upper ceiling with all optional items |

---

## 2. Core Assumptions

| Assumption | Value | Source |
|---|---|---|
| Hourly Rate (Junior FE / Senior BE & INT) | 200,000 VND / 300,000 VND | Class Parameters (ITviec Salary Index) |
| AI Leverage Factor (FE / BE / INT) | 0.35 / 0.7 / 1.0 | Class Parameters |
| Verification Factor (Precise / Vague) | 0.15 / 0.40 | Class Parameters |
| Team Capacity | 360 person-hours | 6 members x 12 weeks x 5 hours/week |
| Financial Contingency on Must Scope | 10% = 3,546,000 VND | Class Parameters |

---

## 3. Execution Priority Seed (Risk-Ordered Task Seed)

Risk Order: 1 = INT (Third-party APIs), 2 = BE (Complex logic/DB), 3 = FE (UI components).

| L3 ID | Layer | Person-hours | Risk Order | Feature |
|---|---|---|---|---|
| FR-02-02-INT | INT | 18.4 | 1 | F-02 (VNPay/MoMo Payment) |
| FR-06-01-INT | INT | 9.2 | 1 | F-06 (Firebase FCM Push) |
| FR-00-01-BE | BE | 6.8 | 2 | F-00 (Auth API) |
| FR-00-02-BE | BE | 3.4 | 2 | F-00 (RBAC Middleware) |
| FR-00-03-BE | BE | 6.8 | 2 | F-00 (CI/CD Pipeline) |
| FR-00-04-BE | BE | 3.4 | 2 | F-00 (Health Check Engine) |
| FR-02-01-BE | BE | 3.4 | 2 | F-02 (Ticket Catalogue API) |
| FR-02-02-BE | BE | 6.8 | 2 | F-02 (Order & QR Engine) |
| FR-02-03-BE | BE | 3.4 | 2 | F-02 (Wallet QR API) |
| FR-03-01-BE | BE | 6.8 | 2 | F-03 (Map Navigation Nodes) |
| FR-04-01-BE | BE | 3.4 | 2 | F-04 (Quest Progress API) |
| FR-04-02-BE | BE | 6.8 | 2 | F-04 (Checkpoint Verification) |
| FR-04-03-BE | BE | 3.4 | 2 | F-04 (Storybook Content API) |
| FR-04-04-BE | BE | 6.8 | 2 | F-04 (Daily Streak Reset Job) |
| FR-06-01-BE | BE | 6.8 | 2 | F-06 (Campaign Scheduler API) |
| FR-00-01-FE | FE | 2.0 | 3 | F-00 (Sign-in UI) |
| FR-02-01-FE | FE | 2.0 | 3 | F-02 (Catalogue UI) |
| FR-02-02-FE | FE | 4.0 | 3 | F-02 (Checkout UI) |
| FR-02-03-FE | FE | 2.0 | 3 | F-02 (Wallet UI) |
| FR-03-01-FE | FE | 8.0 | 3 | F-03 (2.5D Map Screen) |
| FR-04-01-FE | FE | 2.0 | 3 | F-04 (Quest Hub UI) |
| FR-04-02-FE | FE | 4.0 | 3 | F-04 (Camera Scanner UI) |
| FR-04-03-FE | FE | 2.0 | 3 | F-04 (Storybook UI) |
| FR-04-04-FE | FE | 2.0 | 3 | F-04 (Streak UI) |
| FR-06-01-FE | FE | 4.0 | 3 | F-06 (Campaign Admin UI) |

---

## 4. Open Questions for Client Sign-off

| # | Question | Blocking Status |
|---|---|---|
| 1 | Will park management allow manual paper verification for space rental requests during Release 1? | Non-blocking |
| 2 | Is VNPay the sole primary payment gateway for E-Tickets, or must MoMo be supported simultaneously? | Blocking (FR-02-02-INT scope) |
| 3 | Is the GPS radius threshold for QR checkpoint scanning fixed at 50 meters? | Non-blocking |