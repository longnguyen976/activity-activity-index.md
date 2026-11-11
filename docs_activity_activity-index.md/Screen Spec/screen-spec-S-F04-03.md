# Screen Spec: S-F04-03 Check-in Streak

| Field | Value |
|---|---|
| Screen ID | `S-F04-03` |
| Screen name | Check-in Streak |
| Actor | Visitor / GenZ Customer |
| Priority | Must (FR-04-05, FR-04-06, FR-04-08, FR-04-10 to FR-04-16 are Must; FR-04-07 and FR-04-09 are Should) |
| Feature | `F-04 Loyalty & Rewards` (part of F-04 Gamification & Loyalty Engine) |
| Activity diagrams | `AD-UC-05` (Phase 1 Earn Coins, Phase 2 Streak Management, Phase 3 Milestone Reward) |
| Mockup image | `img/S-F04-03.png` |
| Status | Draft |

Document Owner: Đoàn Thị Bảo Ngân | Last Updated: 2026-11-08

## 1. Purpose

**Shown when:** The visitor opens the Rewards tab (or taps a streak push notification) to see their coin balance, today's daily-login reward and streak, check in at the park for activity coins, and track progress toward milestone rewards.

**The user leaves this screen when:** The visitor taps "Scan to Check In" (opens the shared QR scanner on `S-F04-01`), taps "Redeem Coins" (`S-F04-04`), taps "Open Park Map" on a locked major milestone (`S-F03-01`), or goes back to the Main Menu.

**Source trace:** `AD-UC-05` nodes `c1, p1-p3` (daily claim), `c2, p4-p6` (check-in), `f1, p7-p9` (purchase coins, shown in history only), `m1, p10-p16` (milestones), `S2, q1-q3` (end-of-day streak job, shown as streak state and push only).

## 2. Mockup

![S-F04-03](img/S-F04-03.png)

> Sample data only. Amounts and zone names in the image are illustrative.

Layout overview:

- **Top bar:** Back button | title "Check-in Streak" | notification bell.
- **Coin balance card:** Current coin balance and a secondary button "Redeem Coins".
- **Streak card:** Current streak in days, a 7-day strip (one dot per day, today highlighted), and today's daily reward status ("+100 coins claimed today" / "Daily reward already claimed").
- **Check-in card:** Prompt "At Dam Sen Park? Scan a check-in QR to earn coins." and the screen's single primary button "Scan to Check In", which opens the shared scanner on `S-F04-01`.
- **Milestones section:** Two tracks. Normal: 1,000 / 1,500 / 2,000 coins. Major: 5,000 / 15,000 / 25,000 coins, marked "Visit required". A progress bar shows coins left to the next milestone.
- **Recent coin activity:** The latest coin events (daily reward, check-in, purchase, voucher redemption), newest first.
- **Overlays:** M-F04-02 Milestone Reward Unlocked modal.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | `StreakTopBar` | Header | Back button, title "Check-in Streak", notification bell | Yes | Bell opens the shared notification list (route TBD) |
| 2 | `CoinBalanceValue` | Text | `wallet.coin_balance`, format `#,### coins`, returned with the daily claim (FR-04-05 / FR-04-06) | Yes | Non-negative integer; always the server value, never computed on the device |
| 3 | `RedeemCoinsButton` | Secondary button | "Redeem Coins" | Yes | Opens `S-F04-04` |
| 4 | `StreakCountText` | Text | `streak.current_days`, e.g. "4-day streak" (FR-04-05) | Yes | Integer ≥ 0; "Start your streak today" when 0 |
| 5 | `StreakWeekStrip` | List (7 day dots) | `streak.days[]` for the current week: claimed / missed / today / upcoming | Yes | Each dot has a text label (day name + status), not colour alone |
| 6 | `StreakHelpText` | Text | "Open the app every day to keep your streak." (static) | Yes | None |
| 7 | `DailyRewardStatus` | Badge | "+100 coins claimed today" after a claim (FR-04-05); "Daily reward already claimed" on a repeat claim (FR-04-06) | Yes | Shows the result of the daily claim made when the app opened |
| 8 | `StreakEndedBanner` | Banner | "Your streak has ended. Open the app every day to build a new one." | No | Shown once after the end-of-day job reset the streak to 0 (FR-04-08); dismissible |
| 9 | `CheckInPrompt` | Text | "At Dam Sen Park? Scan a check-in QR to earn coins." (static) | Yes | None |
| 10 | `ScanCheckInButton` | Primary button | "Scan to Check In" | Yes | Opens the shared scanner on `S-F04-01` in check-in mode |
| 11 | `NextMilestoneProgress` | Progress bar + text | `coin_balance` vs next threshold, e.g. "250 coins to 1,500" (FR-04-09) | Yes | Hidden when all milestones are achieved |
| 12 | `NormalMilestoneList` | List | Thresholds 1,000 / 1,500 / 2,000 with status Locked / Unlocked / Claimed (FR-04-10, FR-04-11) | Yes | Status shown as text + icon |
| 13 | `MajorMilestoneList` | List | Thresholds 5,000 / 15,000 / 25,000 with status Locked / Visit required / Unlocked / Claimed (FR-04-12 to FR-04-14) | Yes | "Visit required" when the threshold is reached but no verified physical-engagement record exists |
| 14 | `VisitRequiredHint` | Text | "Visit Dam Sen or make a qualifying purchase to unlock this reward." (FR-04-14) | No | Only on a major milestone in "Visit required" status |
| 15 | `OpenParkMapLink` | Text link | "Open Park Map" | No | Only with `VisitRequiredHint`; opens `S-F03-01` |
| 16 | `AlreadyClaimedBadge` | Badge | "Reward already claimed" (FR-04-11) | No | On normal milestones whose entitlement was already generated |
| 17 | `CoinActivityList` | List | `coin_history[]`: amount (+/−), source (Daily reward / Check-in / Purchase / Voucher redemption), date `DD/MM/YYYY HH:mm` | Yes | Newest first; first 10 items, then "Show More" |
| 18 | `ShowMoreHistoryButton` | Text button | "Show More" | No | Hidden when there are no more items |
| 19 | `EmptyStateMessage` | Banner | "You have no coins yet. Open the app every day and check in at the park to start earning." | No | Only when balance = 0 and history is empty |
| 20 | `MilestoneUnlockedModal` (M-F04-02) | Modal | "Normal milestone reward unlocked!" (FR-04-10) or "Major Reward Unlocked!" (FR-04-13), reward name, button "Got It" | No | Opens once per newly unlocked milestone |
| 21 | `FeatureLabel` | Text | "F-04 · Loyalty & Rewards" (mockup annotation only) | No | Not shown in the built app |
| 22 | `BottomNavigation` | Shared app navigation | MySen / Tickets / Rewards / Account, "Rewards" active | Yes | Shared shell; route owner to confirm |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Coin balance, current streak with week strip, today's reward status, "Scan to Check In" button, milestone tracks with progress bar, recent coin activity | Visitor opens the screen and wallet, streak and milestone data load |
| Empty (no data) | Balance "0 coins", "Start your streak today", all milestones Locked, `EmptyStateMessage` instead of the activity list | New visitor with no coin history |
| Loading | Skeleton cards for balance, streak and milestones on first load; small spinner under the list while "Show More" loads | Screen opens; visitor taps "Show More" |
| Error | Load failure: banner "We couldn't load your coins. Check your connection and try again." with "Try Again". Check-in rejected (shown on return from the scanner): toast "Check-in failed. You need to be inside Dam Sen Park to check in." (`AD-UC-05 p6`). The balance never changes on an error | Wallet request fails; Park Check-in System does not confirm the check-in |
| Success / confirmation | Daily reward: toast "+100 Coins! Your streak is now 4 days." (FR-04-05). Check-in: toast "+X Coins" (FR-04-15). Purchase at a partner POS: toast "+X Coins" the next time the app is open (FR-04-16). Milestone reached: M-F04-02 modal (FR-04-10, FR-04-13). Balance and activity list update at once | Daily claim recorded; check-in confirmed; transaction validated; milestone entitlement generated |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Open the screen (the daily claim runs when the app opens) | Loads balance, streak and milestones. First open of the day and not claimed → credits 100 coins, updates the streak and records the claim (FR-04-05). Already claimed → returns the current balance and streak, no extra coins, shows "Daily reward already claimed" (FR-04-06) | stays |
| 2 | Push notification | Tap "Your streak is at risk. Log in today to keep it!" (FR-04-07) | Opens the app on this screen; the daily claim runs as in row 1 | `S-F04-03` |
| 3 | `ScanCheckInButton` | Tap "Scan to Check In" | Opens the shared QR scanner (camera permission handled there) | `S-F04-01` |
| 4 | Shared scanner (`S-F04-01`) | Scan a valid check-in QR | Sends userID, location and timestamp to the Park Check-in System. Confirmed → adds activity coins, returns "+X Coins" (FR-04-15) and re-checks milestones. Not confirmed → rejects, no coins | `S-F04-03` |
| 5 | Coin update (system event) | — (after a daily claim, check-in or purchase) | Compares the balance with the thresholds. Not reached → returns progress to the next milestone (FR-04-09). Normal reached, not claimed → marks it achieved, generates the entitlement and opens M-F04-02 (FR-04-10). Normal already claimed → "Reward already claimed", no duplicate (FR-04-11). Major reached → checks verified physical-engagement records (FR-04-12): verified → premium entitlement and M-F04-02 (FR-04-13); not verified → keeps it locked with `VisitRequiredHint` (FR-04-14) | stays |
| 6 | `MilestoneUnlockedModal` | Tap "Got It" | Closes the modal; the milestone shows "Unlocked" | stays |
| 7 | `OpenParkMapLink` | Tap "Open Park Map" | Opens the park map so the visitor can plan a visit | `S-F03-01` |
| 8 | `RedeemCoinsButton` | Tap "Redeem Coins" | Opens the voucher catalogue with the current balance | `S-F04-04` |
| 9 | `ShowMoreHistoryButton` | Tap "Show More" | Loads the next 10 coin events | stays |
| 10 | `StreakEndedBanner` | Tap close | Hides the banner for this streak reset | stays |
| 11 | Error banner | Tap "Try Again" | Re-requests wallet, streak and milestone data | stays |
| 12 | Back button | Tap | Returns to the previous screen | Main Menu |
| 13 | Screen (not signed in) | Open the screen without a session | Redirects to sign in; returns here after sign in | `S-F00-01` |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only a signed-in visitor can see coins, claim the daily reward or check in. | `AD-UC-05` actor; `S-F00-01` |
| SR-002 | The daily reward is +100 coins, at most once per visitor per day, claimed when the app opens. A repeat claim adds nothing and shows "Daily reward already claimed". | FR-04-05, FR-04-06; BT-01, BT-02 |
| SR-003 | A daily scheduled check sends a "streak at risk" push to a visitor who has not logged in today. If the visitor has still not logged in by the end-of-day deadline, the streak resets to 0 and a streak-reset push is sent. | FR-04-07, FR-04-08; BT-16 to BT-18 |
| SR-004 | Check-in uses the shared QR scanner of `S-F04-01`. Coins are added only after the Park Check-in System confirms the check-in. A rejected check-in adds no coins. | FR-04-15; `AD-UC-05 d2`; BT-03, BT-04 |
| SR-005 | Purchase coins come only from a transaction reported by the Ticketing/POS System and validated by the backend. This screen only shows them; it never adds them itself. | FR-04-16; BT-05, BT-06 |
| SR-006 | Milestones are checked after every coin update. Normal: 1,000 / 1,500 / 2,000. Major: 5,000 / 15,000 / 25,000. | FR-04-09, FR-04-10, FR-04-12 |
| SR-007 | Each normal milestone reward is generated once. A claimed milestone shows "Reward already claimed" and creates nothing new. | FR-04-11; BT-09 |
| SR-008 | A major milestone needs a verified physical-engagement record (park visit or qualifying purchase). Without it the reward stays locked even when the coin threshold is reached. | FR-04-12, FR-04-13, FR-04-14; BT-10, BT-11 |
| SR-009 | Balance, streak and milestone status always come from the server; the app never adds coins locally (prevents double claims from two devices). | FR-04-06, FR-04-11 |
| SR-010 | Numbers use `#,###`; dates use `DD/MM/YYYY HH:mm`; buttons use verb-first labels; one primary button per screen ("Scan to Check In"). | UI Conventions |

## 7. Linked requirements

| FR ID | Priority | What this screen does for it |
|---|---|---|
| FR-04-05 | Must | Shows the +100 coins daily reward, the updated streak and balance |
| FR-04-06 | Must | Shows "Daily reward already claimed" with the current balance and streak |
| FR-04-07 | Should | Opens from the "streak at risk" push |
| FR-04-08 | Must | Shows the streak reset to 0 and the "streak has ended" banner |
| FR-04-09 | Should | Shows the progress bar to the next milestone |
| FR-04-10 | Must | Shows the "Normal milestone reward unlocked" modal |
| FR-04-11 | Must | Shows "Reward already claimed" on a claimed normal milestone |
| FR-04-12 | Must | Shows "Visit required" while the physical-engagement check is not met |
| FR-04-13 | Must | Shows the "Major Reward Unlocked" modal |
| FR-04-14 | Must | Shows the locked prompt with "Open Park Map" |
| FR-04-15 | Must | Starts the QR check-in (shared scanner) and shows "+X Coins" |
| FR-04-16 | Must | Shows purchase coins "+X Coins" in the activity list |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px. All cards stack in one column.
- Mobile: "Scan to Check In" stays near the top of the screen; the week strip scrolls horizontally if it does not fit.
- Tablet / desktop web: two columns, with balance, streak and check-in on the left and milestones and activity on the right; content width capped at 960px.
- Information never carried by colour alone: streak days, milestone status and coin amounts (+/−) always have a text label or icon.
- Coin and streak changes are announced to screen readers (live region). Tap targets at least 48 × 48 dp on mobile; body text contrast at least 4.5:1; the modal traps focus and returns it to the screen on "Got It".

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: The assignment calls this screen "Check-in Streak", but FR-04-05 to FR-04-08 define a daily login streak. Is there a separate streak for park check-ins?] | No | Open (drawn as the login streak) |
| 2 | [NEEDS CLARIFICATION: How many coins does one check-in earn (FR-04-15 "activity-based"), and how many check-ins per day count?] | No | Open |
| 3 | [NEEDS CLARIFICATION: Are milestones compared with the current spendable balance (FR-04-09) or with lifetime coins earned? With the balance, spending coins on vouchers could make a visitor fall below a milestone again.] | Yes (business rule) | Open |
| 4 | [NEEDS CLARIFICATION: What is a "reward entitlement" (FR-04-10) and a "premium reward entitlement" (FR-04-13): a voucher, a gift or a discount? Where does the visitor use it?] | Yes (content) | Open |
| 5 | [NEEDS CLARIFICATION: With Hải Long: when the shared scanner on `S-F04-01` is opened from this screen, does a valid scan return here with "+X Coins", or go to the Storybook `S-F04-02` as in the quest flow?] | No | Open (drawn as return here) |
| 6 | [NEEDS CLARIFICATION: What time zone and cut-off define "end of day" (FR-04-08), and when is the "streak at risk" push sent (FR-04-07)?] | No | Open (assumed 23:59 Asia/Ho_Chi_Minh) |
