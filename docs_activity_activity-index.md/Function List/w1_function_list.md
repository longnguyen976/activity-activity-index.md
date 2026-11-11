# W1 Function List: MySen Platform

Lesson 5 (block W1). L2 = business behavior signed off with EARS criteria and FP type. L3 = work item per layer (FE, BE, INT), sized S = 4h, M = 8h, L = 16h. Effective priority derived from W4.

## L2 functions

| Feature | L2 ID | L2 function name | Actor | EARS acceptance criterion | Source trace | Initial priority | FP type |
|---|---|---|---|---|---|---|---|
| F-00 | FR-00-01 | Sign in & authentication | Visitor, Partner, Staff | WHEN a user submits valid credentials, the system SHALL authenticate and start a role-based session. | Context: SSO / Auth; UC-01 | Must | EQ |
| F-00 | FR-00-02 | Enforce user roles & access | System | IF a user accesses a route outside their assigned role, THEN the system SHALL deny access and redirect to home. | Architecture; UC-01, UC-10 | Must | NC |
| F-00 | FR-00-03 | Automated build & deployment | System | WHEN code is merged to main branch, the pipeline SHALL build, test, and deploy to staging. | Architecture | Must | NC |
| F-00 | FR-00-04 | System monitoring & error alerts | System | IF a critical system failure occurs, THEN the system SHALL log the event and send immediate admin alerts. | UC-10 | Must | NC |
| F-02 | FR-02-01 | Browse E-Ticket catalogue | Visitor | WHEN a visitor opens the ticket page, the system SHALL display available entry and event tickets with prices. | UC-02 | Must | EQ |
| F-02 | FR-02-02 | Ticket checkout & payment | Visitor | WHEN a visitor confirms ticket selection and pays, the system SHALL process payment and generate QR tickets. | UC-02 | Must | EI |
| F-02 | FR-02-03 | View & manage ticket wallet | Visitor | WHEN a visitor opens the wallet, the system SHALL list active, used, and expired QR tickets. | UC-02 | Must | EQ |
| F-03 | FR-03-01 | Display interactive 2.5D map | Visitor | WHEN a visitor opens the map, the system SHALL render 2.5D park layout with interactive POI markers. | UC-03 | Must | EQ |
| F-04 | FR-04-01 | Display Quest Hub & progress | Visitor | WHEN a visitor opens Quest Hub, the system SHALL show active park quests and current checkpoint completion status. | UC-04 | Must | EQ |
| F-04 | FR-04-02 | QR Checkpoint scanner | Visitor | WHEN a visitor scans a valid checkpoint QR, the system SHALL confirm check-in and award quest XP/badges. | UC-04 | Must | EI |
| F-04 | FR-04-03 | Storybook Reader | Visitor | WHEN a checkpoint is unlocked, the system SHALL display the corresponding park lore storybook page. | UC-04 | Must | EQ |
| F-04 | FR-04-04 | Daily Check-in Streak | Visitor | WHEN a visitor logs in daily, the system SHALL update streak count, reward coins, and notify upcoming milestones. | UC-05 | Must | EI |
| F-04 | FR-04-05 | Redeem coins for vouchers | Visitor | WHEN a visitor selects a voucher and confirms redemption, the system SHALL deduct coins and issue a voucher code. | UC-05 | Should | EI |
| F-05 | FR-05-01 | Social Gallery & photo sharing | Visitor | WHEN a visitor uploads a park photo, the system SHALL publish it to the public social feed after validation. | UC-11 | Should | EI |
| F-05 | FR-05-02 | Partner space rental booking | Partner | WHEN a partner submits a space booking form, the system SHALL save the request and notify admin for review. | UC-06 | Should | EI |
| F-06 | FR-06-01 | Manage marketing campaigns | Staff / Admin | WHEN an admin creates/edits a campaign, the system SHALL update campaign status and schedule push notifications. | UC-08 | Must | EI |
| F-06 | FR-06-02 | Analyze customer analytics | Staff / Admin | WHEN an admin opens analytics dashboard, the system SHALL generate occupancy, sales, and quest engagement metrics. | UC-09 | Should | EQ |

---

## L3 work items

| L3 ID | L2 ID | Layer | L3 work item | Size | Base hours | EARS precise? | Hidden work? |
|---|---|---|---|---|---|---|---|
| FR-00-01-FE | FR-00-01 | FE | Sign-in & Register Screen (S-F00-01) | S | 4 | Yes | No |
| FR-00-01-BE | FR-00-01 | BE | Auth API & JWT Token Session Management | M | 8 | Yes | No |
| FR-00-02-BE | FR-00-02 | BE | RBAC Middleware & Route Guards | S | 4 | Yes | Yes |
| FR-00-03-BE | FR-00-03 | BE | CI/CD Pipeline & Automated Staging Script | M | 8 | Yes | Yes |
| FR-00-04-BE | FR-00-04 | BE | Health-check endpoint & alert notification logger | S | 4 | Yes | Yes |
| FR-02-01-FE | FR-02-01 | FE | E-Ticket Catalogue Screen (S-F02-01) | S | 4 | Yes | No |
| FR-02-01-BE | FR-02-01 | BE | Ticket Inventory & Pricing API | S | 4 | Yes | No |
| FR-02-02-FE | FR-02-02 | FE | E-Ticket Checkout Screen (S-F02-02) | M | 8 | Yes | No |
| FR-02-02-BE | FR-02-02 | BE | Order Processing & QR Generation Engine | M | 8 | Yes | No |
| FR-02-02-INT | FR-02-02 | INT | Payment Gateway Integration (VNPay / MoMo) | L | 16 | Yes | No |
| FR-02-03-FE | FR-02-03 | FE | Ticket Wallet Screen (S-F02-03) | S | 4 | Yes | No |
| FR-02-03-BE | FR-02-03 | BE | Wallet Data & Active QR Validation API | S | 4 | Yes | No |
| FR-03-01-FE | FR-03-01 | FE | Interactive 2.5D Map Screen (S-F03-01) | L | 16 | Yes | No |
| FR-03-01-BE | FR-03-01 | BE | POI Coordinates & Route Node Navigation API | M | 8 | Yes | No |
| FR-04-01-FE | FR-04-01 | FE | Quest Hub Screen (S-F04-01 - Tab 1) | S | 4 | Yes | No |
| FR-04-01-BE | FR-04-01 | BE | Active Quests & Progress Calculation API | S | 4 | Yes | No |
| FR-04-02-FE | FR-04-02 | FE | Camera QR Scanner Component (S-F04-01 - Tab 2) | M | 8 | Yes | No |
| FR-04-02-BE | FR-04-02 | BE | Checkpoint Validation & Badge Reward Engine | M | 8 | Yes | No |
| FR-04-03-FE | FR-04-03 | FE | Storybook Reader Screen (S-F04-02) | S | 4 | Yes | No |
| FR-04-03-BE | FR-04-03 | BE | Park Lore & Storybook Content API | S | 4 | Yes | No |
| FR-04-04-FE | FR-04-04 | FE | Check-in Streak Screen (S-F04-03) | S | 4 | Yes | No |
| FR-04-04-BE | FR-04-04 | BE | Daily Streak Tracking & End-of-Day Cron Job | M | 8 | Yes | No |
| FR-04-05-FE | FR-04-05 | FE | Voucher Exchange Screen (S-F04-04) | S | 4 | Yes | No |
| FR-04-05-BE | FR-04-05 | BE | Coin Redemption & Voucher Issuance API | S | 4 | Yes | No |
| FR-05-01-FE | FR-05-01 | FE | Social Gallery Feed Screen (S-F05-01) | M | 8 | Yes | No |
| FR-05-01-BE | FR-05-01 | BE | Photo Post Upload, Like & Moderation API | M | 8 | Yes | No |
| FR-05-02-FE | FR-05-02 | FE | Partner Rental Application Screen (S-F05-02) | S | 4 | Yes | No |
| FR-05-02-BE | FR-05-02 | BE | Rental Request Submission & Workflow API | S | 4 | Yes | No |
| FR-06-01-FE | FR-06-01 | FE | Admin Dashboard - Campaign Tab (S-F06-01) | M | 8 | Yes | No |
| FR-06-01-BE | FR-06-01 | BE | Campaign Management & Scheduler API | M | 8 | Yes | No |
| FR-06-01-INT | FR-06-01 | INT | Firebase Cloud Messaging Push Notification | M | 8 | Yes | No |
| FR-06-02-FE | FR-06-02 | FE | Admin Analytics & Reporting Screen | M | 8 | Yes | No |
| FR-06-02-BE | FR-06-02 | BE | Aggregated Data Insights & Chart Query API | M | 8 | Yes | No |

Total: 17 L2 functions, 33 L3 work items, 236 base hours. Maximum L3 item size: 16 hours.