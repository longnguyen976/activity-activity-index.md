# Screen Spec: S-F04-04 Voucher Exchange

| Field | Value |
|---|---|
| Screen ID | `S-F04-04` |
| Screen name | Voucher Exchange |
| Actor | Visitor / GenZ Customer (external system: F&B Partner POS, which verifies the voucher at the counter) |
| Priority | Should (FR-02-13 to FR-02-17 are Should) |
| Feature | `F-04 Loyalty & Rewards` |
| Activity diagrams | `AD-UC-05` (nodes `c4, c5, p17-p22, f2-f4`) |
| Mockup image | `img/S-F04-04.png` |
| Status | Draft |

Document Owner: Đoàn Thị Bảo Ngân | Last Updated: 2026-11-08

## 1. Purpose

**Shown when:** The visitor taps "Redeem Coins" on `S-F04-03` (or opens Rewards → Vouchers) to exchange coins for an F&B voucher, or to open a voucher they already own and show it at a partner counter.

**The user leaves this screen when:** The visitor taps Back (to `S-F04-03` or the Main Menu), or taps the coin balance to go to `S-F04-03`. Redeeming, viewing a voucher code and refreshing its status all stay on this screen.

**Source trace:** `AD-UC-05` `c4 → p17 → d8 → p18 / p19 → c5 → f2 → p20 → d9 → f3 + p21 / p22 + f4`; sequence `SEQ:VCH-01` Phase 2 (exchange) and Phase 3 (use at the outlet). In W1 these UC-05 functions are numbered FR-02-13 to FR-02-17.

## 2. Mockup

![S-F04-04](img/S-F04-04.png)

> Sample data only. Partner names, prices and stock in the image are placeholders.

Layout overview:

- **Top bar:** Back button | title "Voucher Exchange" | coin balance chip (e.g. "1,250 coins").
- **Segment control:** "Exchange" | "My Vouchers".
- **Exchange tab:** Scrollable list of voucher cards. Each card: partner name and zone, reward name, cost in coins, validity after redemption, vouchers left, and a "Redeem" button. A card the visitor cannot afford shows "You need X more coins" and a disabled button.
- **My Vouchers tab:** List of owned vouchers grouped by status: Active, Redeemed, Expired. Tapping an Active voucher opens the voucher code.
- **Overlays:** M-F04-03 Confirm Redemption dialog; M-F04-04 Voucher Code sheet.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | `VoucherTopBar` | Header | Back button, title "Voucher Exchange" | Yes | None |
| 2 | `CoinBalanceChip` | Badge / Button | `wallet.coin_balance`, format `#,### coins` | Yes | Server value; refreshed after every redemption; tap opens `S-F04-03` |
| 3 | `VoucherSegment` | Tab toggle | "Exchange" \| "My Vouchers" | Yes | Opens on "Exchange" unless the entry point says otherwise |
| 4 | `VoucherCatalogueList` | List | `reward_catalogue[]`: active F&B rewards with partner stock (FR-02-13) | Yes (Exchange tab) | Only active rewards; sorted by cost, lowest first; out-of-stock rewards last |
| 5 | `PartnerName` | Text | `reward.partner_name` + `reward.zone_name` | Yes | None |
| 6 | `RewardName` | Text | `reward.name`, e.g. "Iced Milk Coffee (M)" | Yes | Maximum 2 lines, then "…" |
| 7 | `RewardImage` | Image | `reward.image_url` | No | Alt text = reward name; placeholder icon when missing |
| 8 | `CoinCost` | Text | `reward.coin_cost`, format `#,### coins` | Yes | Positive integer |
| 9 | `ValidityText` | Text | "Valid for N days after redemption" from `reward.validity_days` | Yes | None |
| 10 | `StockText` | Badge | "N left" from partner stock (FR-02-13); "Out of stock" when 0 | No | Text label, not colour only |
| 11 | `RedeemButton` | Secondary button (per card) | "Redeem" | Yes | Disabled when balance < cost or stock = 0 |
| 12 | `NotEnoughCoinsText` | Text | "You need X more coins" (X = cost − balance) | No | Only when balance < cost (`AD-UC-05 p18`) |
| 13 | `ConfirmRedemptionDialog` (M-F04-03) | Modal | "Redeem [reward] for [cost] coins? Your balance after this: [balance − cost] coins." Buttons "Confirm Redemption" (primary) and "Cancel" | Yes | "Confirm Redemption" is disabled after the first tap until a result returns (no double submit) |
| 14 | `MyVoucherList` | List | `vouchers[]` owned by the visitor (FR-02-14), grouped as Active / Redeemed / Expired | Yes (My Vouchers tab) | Active first, sorted by expiry date, soonest first |
| 15 | `VoucherStatusBadge` | Badge | Active / Redeemed / Expired (`voucher.status`: ACTIVE, REDEEMED, expired) | Yes | Text label, not colour only |
| 16 | `VoucherExpiryText` | Text | "Expires DD/MM/YYYY HH:mm" or "Redeemed DD/MM/YYYY HH:mm" | Yes | None |
| 17 | `VoucherCodeSheet` (M-F04-04) | Bottom sheet | Voucher QR + text code `voucher.code`, partner, outlet, reward, expiry, "Show this code at the partner counter", button "Refresh Status" (FR-02-14) | Yes | Only for ACTIVE vouchers; QR comes from the server, never generated on the device |
| 18 | `RefreshStatusButton` | Secondary button | "Refresh Status" (inside M-F04-04) | No | Reads the latest status; never re-issues a voucher |
| 19 | `EmptyCatalogueMessage` | Banner | "No vouchers are available right now. Check back soon!" | No | Exchange tab with no active rewards |
| 20 | `EmptyWalletMessage` | Banner | "You have no vouchers yet." + text button "Browse Vouchers" | No | My Vouchers tab with no vouchers |
| 21 | `FeatureLabel` | Text | "F-04 · Loyalty & Rewards" (mockup annotation only) | No | Not shown in the built app |
| 22 | `BottomNavigation` | Shared app navigation | MySen / Tickets / Rewards / Account, "Rewards" active | Yes | Shared shell; route owner to confirm |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Exchange tab: balance chip and voucher cards; cards the visitor can afford have an active "Redeem" button, others show "You need X more coins". My Vouchers tab: owned vouchers with status badges | Screen opens and the catalogue, balance and voucher list load |
| Empty (no data) | Exchange tab: `EmptyCatalogueMessage`. My Vouchers tab: `EmptyWalletMessage` with "Browse Vouchers" | No active rewards; visitor owns no vouchers |
| Loading | Skeleton cards on first load; in M-F04-03, "Confirm Redemption" shows a spinner while eligibility, stock, coin deduction and issue run; in M-F04-04, "Refresh Status" shows a spinner | Opening the screen or a tab; confirming a redemption; refreshing a voucher |
| Error | Not enough coins: toast "You don't have enough coins for this voucher." (`AD-UC-05 p18`). Out of stock: toast "This voucher just ran out. Please choose another one." (FR-02-13). No coins are taken in either case. Issue failed after the deduction: toast "We couldn't issue your voucher. Your X coins have been returned." (FR-02-15). Load failure: banner "We couldn't load vouchers. Check your connection and try again." with "Try Again" | Balance < cost; partner stock = 0; issue fails after the deduction; network or server error |
| Success / confirmation | Toast "Voucher redeemed! You can find it in My Vouchers." The balance chip drops by the cost, the screen switches to "My Vouchers" and opens M-F04-04 for the new voucher (FR-02-14). After the partner POS accepts it, the voucher shows "Redeemed" on refresh (FR-02-16) | Coins deducted and voucher issued; POS redemption accepted |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Open the screen | Loads balance, active rewards with partner stock (FR-02-13) and the visitor's vouchers (FR-02-14) | stays |
| 2 | `VoucherSegment` | Tap "Exchange" or "My Vouchers" | Switches the list | stays |
| 3 | `RedeemButton` | Tap "Redeem" | Opens M-F04-03 with the reward, cost and balance after redemption | stays (modal) |
| 4 | `ConfirmRedemptionDialog` | Tap "Cancel" | Closes the dialog; nothing is deducted | stays |
| 5 | `ConfirmRedemptionDialog` | Tap "Confirm Redemption" | F-04 checks the balance against the cost (`AD-UC-05 p17`). Not enough → not-enough-coins error, no deduction (`p18`). Eligible → checks voucher availability with the F&B partner service before any deduction (FR-02-13); out of stock → error, no deduction. Available → F-04 deducts the coins, then a unique voucher code / QR with an expiry is issued, stored as ACTIVE and returned (FR-02-14). If issue fails after the deduction → coins restored, redemption marked FAILED, no active voucher (FR-02-15) | stays (dialog closes; M-F04-04 opens on success) |
| 6 | `MyVoucherList` | Tap an Active voucher | Opens M-F04-04 with the voucher QR and text code | stays (modal) |
| 7 | `VoucherCodeSheet` | Visitor shows the code at the partner counter | POS submits the code (`AD-UC-05 f2, p20`). Valid → voucher set from ACTIVE to REDEEMED in one step and the POS applies the offer (FR-02-16). Not existing, not ACTIVE, expired, wrong partner / outlet or already redeemed → POS rejects it with the reason; status does not change (FR-02-17) | stays |
| 8 | `RefreshStatusButton` | Tap "Refresh Status" | Reads the latest status; a Redeemed voucher closes the sheet and moves to "Redeemed" | stays |
| 9 | `EmptyWalletMessage` | Tap "Browse Vouchers" | Switches to the "Exchange" tab | stays |
| 10 | Error banner | Tap "Try Again" | Re-requests catalogue, balance and vouchers | stays |
| 11 | `CoinBalanceChip` | Tap | Opens coins, streak and milestones | `S-F04-03` |
| 12 | Back button | Tap | Returns to the previous screen | `S-F04-03` or Main Menu |
| 13 | Screen (not signed in) | Open the screen without a session | Redirects to sign in; returns here after sign in | `S-F00-01` |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only a signed-in visitor can redeem coins or see their vouchers. | `AD-UC-05` actor; `S-F00-01` |
| SR-002 | A redemption needs balance ≥ cost. If not, nothing is deducted and the visitor sees how many coins are missing. | `AD-UC-05 d8`; BT-12 |
| SR-003 | Voucher availability is checked with the F&B partner service **before** any coin is deducted. Out of stock means no deduction. | FR-02-13 |
| SR-004 | A voucher is issued only after F-04 confirms the coin deduction. Each voucher has a unique code / QR and an expiry date and is stored as ACTIVE. | FR-02-14; BT-13 |
| SR-005 | If issue fails after the coins were deducted, the same coins are restored and the redemption is marked FAILED. No active voucher is created. | FR-02-15 |
| SR-006 | One tap of "Confirm Redemption" creates at most one redemption, even if the visitor taps again or the network retries. | FR-02-14, FR-02-15 |
| SR-007 | A voucher is single-use. The POS change from ACTIVE to REDEEMED is atomic, so the same code cannot be accepted twice. | FR-02-16; BT-14 |
| SR-008 | The POS rejects a voucher that does not exist, is not ACTIVE, is expired, belongs to another partner / outlet, or was already redeemed, and returns the reason. Its status does not change and no coins are returned. | FR-02-17; BT-15 |
| SR-009 | The voucher QR and code come from the server. The QR is not shown for Redeemed or Expired vouchers. | FR-02-14 |
| SR-010 | Coin amounts use `#,### coins`; dates use `DD/MM/YYYY HH:mm`; buttons use verb-first labels; a modal is used only to confirm the critical "spend coins" action. | UI Conventions |

## 7. Linked requirements

| FR ID | Priority | What this screen does for it |
|---|---|---|
| FR-02-13 | Should | Shows partner stock; checks availability before any coin deduction; shows the out-of-stock message |
| FR-02-14 | Should | After the F-04 coin deduction, issues and shows the voucher code / QR with its expiry in My Vouchers |
| FR-02-15 | Should | Restores coins and shows the failure message if issue fails after the deduction |
| FR-02-16 | Should | Shows the code for POS redemption; shows "Redeemed" after the POS accepts it |
| FR-02-17 | Should | Keeps an invalid, expired, wrong-outlet or used voucher rejected at the counter |

> In W1 these five rows trace to `UC:UC-05` and `SEQ:VCH-01`; they are numbered under F-02 in the Function List.

## 8. Responsive and accessibility notes

- Smallest supported width: 360px. Voucher cards are full width; the balance chip stays in the top bar.
- Mobile: M-F04-03 opens as a centred dialog; M-F04-04 opens as a full-height bottom sheet with screen brightness raised so the POS can scan the QR.
- Tablet / desktop web: 2-column voucher grid with content width capped at 960px; M-F04-04 opens as a centred dialog.
- Information never carried by colour alone: voucher status, "Out of stock" and the disabled "Redeem" always have text.
- The text code is always shown under the QR so staff can type it in. The QR alt text names the reward, not the code. Tap targets at least 48 × 48 dp; body text contrast at least 4.5:1; dialogs trap focus and return it to the "Redeem" button that opened them.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: What is the reward catalogue (partners, items, coin costs, stock, validity days)? The values in the mockup are placeholders.] | No | Open |
| 2 | [NEEDS CLARIFICATION: Does an expired, unused voucher return any coins?] | No | Open (drawn as no refund) |
| 3 | [NEEDS CLARIFICATION: Should the app also show the POS rejection reason (FR-02-17), or only the POS?] | No | Open (drawn as POS only; the app status stays unchanged) |
| 4 | [NEEDS CLARIFICATION: Is there a limit on vouchers per visitor per day or per reward?] | No | Open |
