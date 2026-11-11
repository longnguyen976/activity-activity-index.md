# Screen List: MySen Platform

## 1. Screen Inventory

| Screen ID | Screen Name | Actor | Feature | Shown When (Purpose) | Entry Points | Use Cases / FRs Served | Priority | Screen Spec File | Owner |
|---|---|---|---|---|---|---|---|---|---|
| S-F00-01 | Sign In | Visitor / GenZ Customer, Partner, Staff | F-00 | Signing in to access personalized features & roles | App launch / Session expired | UC-01 / FR-00-01 | Must | screens/screen-spec-S-F00-01.md | Lê Thành Hiệp |
| S-F01-01 | User Profile | Visitor / GenZ Customer | F-01 | Viewing & editing personal profile, settings & badges | S-F04-03, S-F05-01 | UC-01 / FR-01-01 | Must | screens/screen-spec-S-F01-01.md | Lê Thành Hiệp |
| S-F02-01 | E-Ticket Catalogue | Visitor / GenZ Customer | F-02 | Browsing & selecting park entry & event tickets | Main Navigation -> Tickets | UC-02 / FR-02-01 | Must | screens/screen-spec-S-F02-01.md | Nguyễn Phạm Nguyệt Minh |
| S-F02-02 | E-Ticket Checkout | Visitor / GenZ Customer | F-02 | Reviewing order details & making payment | S-F02-01 | UC-02 / FR-02-02 | Must | screens/screen-spec-S-F02-02.md | Nguyễn Phạm Nguyệt Minh |
| S-F02-03 | Ticket Wallet | Visitor / GenZ Customer | F-02 | Viewing active & redeemed park ticket QRs | S-F02-02, Main Navigation | UC-02 / FR-02-03 | Must | screens/screen-spec-S-F02-03.md | Nguyễn Phạm Nguyệt Minh |
| S-F03-01 | Interactive 2.5D Map | Visitor / GenZ Customer | F-03 | Engaging with interactive park map & navigation | Main Navigation -> Map, S-F04-03 | UC-03 / FR-03-01 | Must | screens/screen-spec-S-F03-01.md | Nguyễn Thị Phúc An |
| S-F04-01 | Quest Hub & QR Scanner | Visitor / GenZ Customer | F-04 | Viewing active quests, progress & scanning checkpoint QR | Main Nav, S-F04-03, physical QR | UC-04 / FR-04-01, FR-04-02 | Must | screens/screen-spec-S-F04-01.md | Nguyễn Hải Long |
| S-F04-02 | Storybook Reader & Badge Reward | Visitor / GenZ Customer | F-04 | Displayed immediately after valid checkpoint QR scan | S-F04-01 (Scan Success) | UC-04 / FR-04-03, FR-04-04 | Must | screens/screen-spec-S-F04-02.md | Nguyễn Hải Long |
| S-F04-03 | Check-in Streak | Visitor / GenZ Customer | F-04 | Viewing daily-login streak, coin balance & checking in | Main Nav, streak push, S-F04-01/04 | UC-05 / FR-04-05 to FR-04-16 | Must | screens/screen-spec-S-F04-03.md | Đoàn Thị Bảo Ngân |
| S-F04-04 | Voucher Exchange | Visitor / GenZ Customer | F-04 | Exchanging coins for F&B vouchers & viewing owned vouchers | S-F04-03 (Redeem Coins), Main Nav | UC-05 / FR-02-13 to FR-02-17 | Should | screens/screen-spec-S-F04-04.md | Đoàn Thị Bảo Ngân |
| S-F05-01 | Social Gallery | Visitor / GenZ Customer | F-05 | Browsing, liking & sharing park visitor photos | Main Navigation -> Social | UC-11 / FR-05-01 | Should | screens/screen-spec-S-F05-01.md | Trần Hương Ly |
| S-F05-02 | Partner Rental & Booking | Partner / Commercial | F-05 | Submitting space rental & booking requests | S-F00-01 (Partner Sign-in) | UC-06 / FR-05-02 | Should | screens/screen-spec-S-F05-02.md | Trần Hương Ly |
| S-F06-01 | Admin Dashboard | Staff / Admin | F-06 | Managing campaigns, viewing analytics & system roles | S-F00-01 (Admin Sign-in) | UC-08, UC-09, UC-10 / FR-06-01 | Must | screens/screen-spec-S-F06-01.md | Lê Thành Hiệp |

---

## 2. Modals & Overlays Inventory

| Modal ID & Name | Opened From Screen | Purpose |
|---|---|---|
| M-F04-01: Badge Unlocked Modal | S-F04-02 | Display unlocked zone achievement badge & +100 XP animation |
| M-F04-02: Milestone Reward Unlocked Modal | S-F04-03 | Tells the visitor a normal or major milestone reward was unlocked after a coin update |
| M-F04-03: Confirm Redemption Dialog | S-F04-04 | Confirms spending coins on a voucher (shows cost and balance after) before any deduction |
| M-F04-04: Voucher Code Sheet | S-F04-04 | Shows the QR and text code of an Active voucher to present at the F&B partner POS |