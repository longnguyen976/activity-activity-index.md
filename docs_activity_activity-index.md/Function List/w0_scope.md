# W0 Scope: MySen Platform

Lesson 5 (Function List workshop, block W0). Inputs: CVCA, Context Diagram, Use Case Diagram, System Architecture at v2.

## Table 1. Use case inventory

| Use case ID | Use case name | Primary actor | Assigned feature | Login required |
|---|---|---|---|---|
| UC-01 | Register & Log in | Visitor, Partner, Staff | F-00 | No |
| UC-02 | Purchase E-Tickets & Manage Wallet | Visitor / GenZ Customer | F-02 | Yes |
| UC-03 | Interactive Map & Navigation | Visitor / GenZ Customer | F-03 | Yes |
| UC-04 | Gamification & Interactive Park Challenges | Visitor / GenZ Customer | F-04 | Yes |
| UC-05 | Earn Points & Redeem Loyalty Rewards | Visitor / GenZ Customer | F-04 | Yes |
| UC-06 | Partner Space Rental & Booking | Partner / Commercial | F-05 | Yes (Partner) |
| UC-08 | Manage Marketing & Promotion Campaigns | Staff / Admin | F-06 | Yes (Staff/Admin) |
| UC-09 | Analyze Customer Data & Insights | Staff / Admin | F-06 | Yes (Staff/Admin) |
| UC-10 | Manage & Maintain System Infrastructure | Staff / Admin | F-06 | Yes (Staff/Admin) |
| UC-11 | Social Gallery & Photo Sharing | Visitor / GenZ Customer | F-05 | Yes |

---

## Table 2. Feature list

| Feature ID | Feature name (business noun) | Scope in one sentence | Primary actors served | Owner | Use cases covered |
|---|---|---|---|---|---|
| F-00 | Platform & Authentication | Core authentication, authorization, role enforcement, and system infrastructure monitoring. | Visitor, Partner, Staff | Lê Thành Hiệp | UC-01, UC-10 |
| F-02 | E-Ticketing & Wallet | E-ticket browsing, purchasing via payment gateways, QR ticket generation, and wallet management. | Visitor / GenZ Customer | Nguyễn Phạm Nguyệt Minh | UC-02 |
| F-03 | Interactive Map & Navigation | 2.5D interactive park navigation, POI location search, and real-time route guidance. | Visitor / GenZ Customer | Nguyễn Thị Phúc An | UC-03 |
| F-04 | Gamification & Loyalty | Park check-in quests, QR scanning, storybook reader, daily streak tracking, and coin redemption for vouchers. | Visitor / GenZ Customer | Nguyễn Hải Long & Đoàn Thị Bảo Ngân | UC-04, UC-05 |
| F-05 | Social & Partner Ecosystem | Visitor photo sharing gallery and commercial space booking/rental request management for partners. | Visitor, Partner | Trần Hương Ly | UC-06, UC-11 |
| F-06 | Admin Operations & Analytics | Marketing campaign management, targeted push notifications, customer data analytics, and role assignment. | Staff / Admin | Lê Thành Hiệp | UC-08, UC-09 |

Feature count: 6 (allowed 3 to 6): PASS. Coverage against CVCA: all primary actors (Visitor, Partner, Staff/Admin) are served.

---

## Table 3. Won't-have (recorded scope exclusions)

| Excluded from scope | How it surfaced | Reason for exclusion |
|---|---|---|
| AR (Augmented Reality) Park Experience | Student / Stakeholder meeting | High development complexity and hardware requirement; deferred to Release 2. |
| Third-party Ticket Reseller API | Commercial Partner inquiry | Initial release focuses direct-to-consumer sales; B2B API scheduled for future phase. |
| Automatic Cash Refund for Tickets | User survey | Refunds require manual verification by admin; auto-refund postponed for financial risk management. |
| Native Desktop Application | Team discussion | Admin portal runs entirely on web responsive layout; native app not required. |

---

## Table 4. Data functions (Function Point Analysis)

| Data group | ILF / EIF | Why | Function points |
|---|---|---|---|
| User Accounts & Profiles | ILF | User profile, authentication credentials, roles, and streak data maintained by MySen | 10 |
| Tickets & Transactions | ILF | Ticket inventory, purchases, QR codes, and payment transaction logs | 10 |
| Map POIs & Route Nodes | ILF | 2.5D map coordinates, park zones, attraction details, and navigation nodes | 7 |
| Quests & Vouchers | ILF | Challenge checkpoints, user quest progress, coin balance, and redeemed vouchers | 10 |
| Posts & Media Gallery | ILF | Shared photo posts, likes, comments, and partner rental applications | 7 |
| Payment Gateway (VNPay/MoMo) | EIF | Read-only payment execution and status callback response from external gateway | 5 |
| **Total data-function points** | | | **49** |

---

## Business Rules

| BR | Rule |
|---|---|
| BR-01 | A visitor must have an active signed-in account to check in at checkpoints or redeem vouchers. |
| BR-02 | Checkpoint QR codes are valid only when scanned within the park geographical boundary (GPS radius check). |
| BR-03 | Daily check-in streak resets to 0 if a user misses a consecutive 24-hour log-in window (end-of-day reset). |
| BR-04 | A voucher can only be redeemed if the user's current coin balance is greater than or equal to the voucher coin cost. |
| BR-05 | Space rental requests submitted by partners must be approved by an Admin before stall allocation. |