# Screen Spec: S-F02-02 E-Ticket Checkout

| Field | Value |
|---|---|
| Screen ID | `S-F02-02` |
| Screen name | E-Ticket Checkout |
| Actor | Visitor / GenZ Customer |
| Priority | Must |
| Feature | `F-02` E-Ticketing & Commerce |
| Activity diagrams | `AD-UC-02A.md` (purchase; proposed `UC-02A` within W0 `UC-02`) |
| Mockup image | `img/S-F02-02.png` |
| Owner | Nguyễn Phạm Nguyệt Minh |
| Status | Draft specification with complete template; pending team source/policy approval |

## 1. Purpose

**Shown when:** A visitor enters Checkout with ticket type(s) and quantities, reviews the payable amount, confirms a PENDING order and selects MoMo/VNPay to pay.

**The user leaves this screen when:** Verified payment **and** successful QR ticket issuance allow **View My Tickets** → `S-F02-03`, or the visitor returns to `S-F02-01` before order commit or to start another purchase after expiry.

**Distinct internal stages** (one screen, not different Screen IDs): `ORDER_REVIEW` → `PENDING_PAYMENT` (visual label; authoritative **Order** enum is `PENDING`) → `PROVIDER_HANDOFF` → `VERIFYING_PAYMENT` → `TICKET_READY` / `PAYMENT_NOT_COMPLETED` / `ORDER_EXPIRED` / `TICKET_PROCESSING`. These are UI states, **not** additional approved backend enums. The provider's hosted payment page is outside MySen.

**Source trace:** `AD-UC-02A` actor `a3/a4/a5`, system `s2–s10`, outcomes `d1/d2/d3`, external payment `g1`, timer `t1`; `FR-02-02`–`FR-02-09`. **The timer runs independently 15 minutes after order creation.**

## 2. Mockup

![S-F02-02 E-Ticket Checkout](img/S-F02-02.png)

> One representative stage is illustrated. Loading, provider handoff, failure, expiration and success use the **same screen**, with conditional controls described below. Example Order ID and amount placeholders are not real payments.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | `CheckoutHeader` | Header / Back button | “Checkout”; app back | Yes | Back allowed safely before payment; when payment in progress use team-approved safe-exit behavior |
| 2 | `CheckoutFeatureLabel` | Text | “F-02 · Order review & payment” (mockup annotation) | No | Static / non-business |
| 3 | `OrderSummary` | Card / list | Ticket name, quantity, price, server-computed total from W0 `Order, ticket & booking` / `Ticket catalogue` | Yes | Revalidate currently sellable items and quantities in `FR-02-02` |
| 4 | `PayableAmount` | Text | Server-calculated `order.amount` (VND) | Yes | Must correspond to exact Order ID sent to gateway |
| 5 | `OrderIdText` | Text | System-generated `order_id` | After confirmation | Never use a UI-generated ID |
| 6 | `OrderStatusText` | Status text | **Order** only: W1 `PENDING`, `PAID`, `EXPIRED`; no fake Order FAILED enum | Conditional | Read authoritative server order state |
| 7 | `PaymentStatusText` | Status text | **Payment** outcome separately: verifying / `SUCCESS`, `FAILED`, `CANCELLED` (provider response) | Conditional | User-facing text may differ, never rely on redirect alone |
| 8 | `ConfirmOrderButton` | Primary button | “Confirm Order” in review stage | Conditional | Valid items, positive quantities, total revalidated; idempotent submit required |
| 9 | `PaymentMethodOptions` | Radio group | MoMo / VNPay | After PENDING order | Exactly one selected; provider must be supported/configured |
| 10 | `PayWithProviderButton` | Primary button | “Pay with MoMo” or “Pay with VNPay” | After provider choice | Existing unexpired PENDING order, exact amount, no duplicate in-flight submit |
| 11 | `PaymentSecurityNote` | Info panel | “Payment success is verified by MySen before an e-ticket is issued.” | Yes | Must not promise success from user action |
| 12 | `PaymentHandoffBanner` | Status / overlay | “Opening secure payment page…”; server-returned provider URL/QR | Conditional | Only trusted provider redirect/QR from `FR-02-03` |
| 13 | `PaymentResultBanner` | Banner | Verifying/failed/cancelled/expired/issued/late paid/issuing QR status messages | Conditional | Derived from order + provider + ticket status; no terminal claim before verified state |
| 14 | `OrderExpiryInfo` | Text | 15 minutes since `order.created_at`, **if** team permits a countdown UI | Conditional | Display source server time, not client-only timer; 15-min backend rule still applies without visual countdown |
| 15 | `RetryPaymentButton` | Secondary button | “Try Again” after failed/cancelled | Conditional | Must check unexpired order and **unapproved retry policy**; no automatic order reuse |
| 16 | `StartNewPurchaseButton` | Secondary button | “Start New Purchase” when expired | Conditional | Must start new order flow, never revive EXPIRED |
| 17 | `ViewTicketsButton` | Primary button | “View My Tickets” | Only when PAID and ticket issued | Must confirm ticket/QR record exists |
| 18 | `RefreshStatusButton` | Secondary button | “Check Status” if provider result delayed | Conditional | Read-only status refresh; no duplicate initiation |
| 19 | `BottomNavigation` | Shared app navigation | MySen / Tickets / Account (if retained by team shell) | Conditional | F-00/F-01 routes owned by team, not local F-02 IDs |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Review selected items, amount and **Confirm Order**. After successful order creation (`PENDING`) this same screen shows Order ID, provider radio options and **Pay with…** action. | Valid selection passed from Catalogue; `FR-02-02` order created. |
| Empty (no data) | “No tickets selected. Browse tickets to continue.”; no order/payment action. | Missing/cleared selection or deep link without eligible order. |
| Loading | “Creating order…”, “Opening payment…”, or “Verifying payment…”; action disabled; no QR. | `FR-02-02/03`, redirect-return, callback verification or state refresh in progress. |
| Error | Separate conditions: order validation failure; gateway initiation fails; provider `FAILED/CANCELLED`; independent `EXPIRED` after 15 min; invalid/late callback requires status review. Specific copy and retry/restart action. | `FR-02-02/03/05/06/07`; AD `d1` rejected / `d2` failure / `d3` expiry. |
| Success / confirmation | For `PAID` **while issuance pending**, display “Payment confirmed. Your ticket is being prepared.”; only after `FR-02-08/09` succeeds show “Your e-ticket is ready” and View My Tickets. | First valid SUCCESS callback commits PAID once and ticket issuance confirms. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Entry from Catalogue | Open Checkout with selection | Revalidate selection/current catalogue; show summary or validation error. | `S-F02-02` |
| 2 | `ConfirmOrderButton` | Tap Confirm Order | Atomically create **one** server `PENDING` order with unique Order ID, items, price and visitor association; prevent accidental duplicate submissions. | stays |
| 3 | `PaymentMethodOptions` | Select MoMo or VNPay | Record chosen configured provider without yet treating order as paid. | stays |
| 4 | `PayWithProviderButton` | Tap Pay with chosen gateway | Invoke `FR-02-03` for the **exact** PENDING order and server amount; open trusted external provider URL or QR. | stays (external handoff) |
| 5 | External provider | Customer authorizes/cancels/returns | Show verifying state, fetch authoritative backend result; do not treat return URL as SUCCESS. | `S-F02-02` |
| 6 | Gateway webhook (no user action) | Received by MySen | Validate authenticity, order ID, amount, payment status and replay (`FR-02-07`); valid SUCCESS/PENDING → PAID **once** (`FR-02-04`). | stays |
| 7 | `PaymentResultBanner` | Observe successful server state | If PAID but issuance pending, show processing; when issued records exist, enable View My Tickets. | stays |
| 8 | `ViewTicketsButton` | Tap View My Tickets | Fetch visitor-linked issued ticket record with authorized QR. | `S-F02-03` |
| 9 | Failed/cancelled result | Customer taps Try Again | Only if server permits a new payment attempt and order remains eligible; **same Order ID behavior TBD**. Otherwise offer a new purchase. | `S-F02-02` (conditional) |
| 10 | `StartNewPurchaseButton` | Tap Start New Purchase | Return to ticket selection for a **new** order; no EXPIRED order can issue ticket. | `S-F02-01` |
| 11 | `RefreshStatusButton` | Tap Check Status | Read-only query of payment/order/issuance; does not trigger a duplicate charge. | stays |
| 12 | Expiry job (no user action) | 15 min after order creation | If still unpaid PENDING without verified successful callback, mark EXPIRED; no ticket issuance. | stays |
| 13 | Invalid / duplicate callback (no user action) | Received by MySen | Reject / acknowledge safely without duplicate paid transition or ticket. Wrong-order/amount never marks PAID. | stays |
| 14 | Valid late SUCCESS after EXPIRED | Received by MySen | Do **not** issue a ticket or silently mark PAID; show generic review status until finance policy is approved. | stays |
| 15 | `CheckoutHeader` | Tap Back before order confirmed | Return to selection with local selected items; once order exists follow safe-exit policy. | `S-F02-01` (if safe) |
| 16 | `BottomNavigation` | Navigate through shared app shell | Follow shared navigation policy (pending); order state remains backend-owned. | stays (shared route TBD) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| `SR-F02-02-01` | Only valid ticket selections create a unique PENDING order with authoritative total. | `FR-02-02`; `AD-UC-02A s2` |
| `SR-F02-02-02` | A payment transaction refers to the exact existing order and amount. | `FR-02-03`; `AD-UC-02A s3` |
| `SR-F02-02-03` | Payment SUCCESS is established by authenticated provider webhook / server verification, **not** by a user returning from checkout. | `FR-02-04/07`; `AD-UC-02A s4/s5` |
| `SR-F02-02-04` | Verify callback origin/signature, transaction match and replay protection; duplicate callback cannot create duplicate ticket/payment effects. | `FR-02-07`; AD `d1` invalid/duplicate branch |
| `SR-F02-02-05` | Provider `FAILED/CANCELLED` leads to failure guidance and **no ticket issuance**; it is a payment outcome, not necessarily an approved Order FAILED state. | `FR-02-05`; `AD-UC-02A s8` |
| `SR-F02-02-06` | Unpaid PENDING expires 15 minutes after **order creation** without verified success; expired order cannot issue ticket. | `FR-02-06`; `AD-UC-02A t1/d3/s10` |
| `SR-F02-02-07` | A PAID order's purchased ticket(s) and protected QR(s) are issued only after valid verified payment; no duplicate issuance. | `FR-02-08/09`; `AD-UC-02A s6/s7` |
| `SR-F02-02-08` | A valid late SUCCESS after EXPIRED triggers unresolved financial reconciliation; UI must not claim a refund or resurrect the order. | `AD-UC-02A` §6 Q2 |
| `SR-F02-02-09` | Retry on failed/cancelled payment is provisional; no silent reuse of an Order ID before approval. | `AD-UC-02A` §6 Q3 |
| `SR-F02-02-10` | Server PAID with pending QR issuance shows **processing** rather than a scannable ticket; duplicate issuance prevented by backend. | UC-02 Sequence issuance-failure branch; `FR-02-08/09` |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| `FR-02-02` | Confirm selection, create PENDING order and show unique Order ID/amount. |
| `FR-02-03` | Select MoMo/VNPay, initiate payment and hand off to provider. |
| `FR-02-04` | Show PAID only after backend authenticates a matching SUCCESS webhook. |
| `FR-02-05` | Show FAILED/CANCELLED payment result and permitted recovery without issuing ticket. |
| `FR-02-06` | Show EXPIRED from independent 15-minute unpaid-order process. |
| `FR-02-07` | Correct UI effect from secure/idempotent callback behavior; backend-only logic is not an extra screen. |
| `FR-02-08` | Show issuance-processing/ready status and View My Tickets link only after issued ticket exists. |
| `FR-02-09` | Depend on secure QR generation but display QR itself only in Ticket Wallet. |

## 8. Responsive and accessibility notes

- Smallest supported width: proposed **360 CSS px** pending team agreement; summary, provider options and status messages stack.
- Label the provider radio group, status updates and Order ID; use `aria-live`-equivalent behavior for asynchronous state changes.
- Body contrast ≥4.5:1, touch targets ≥24×24 CSS pixels, status conveyed in text and never colour alone (Lesson 8 template).
- Provider return flow should retain a visible path back to MySen; actual deep-link/browser configuration is an open interface question.
- Never place unapproved real prices, refund assurances or timeout promises in the mockup; 15-minute business rule is backed by W1, but time remaining depends on server time.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Which single official order enum is used: W1 `PENDING` or sequence alias `PENDING_PAYMENT`? | Yes — data model | Open; draft uses W1 `PENDING` |
| 2 | What happens when genuine provider SUCCESS arrives after server EXPIRED? Finance, reconciliation, reversal and customer message? | Yes — financial integrity | Open; no ticket/refund automatically claimed |
| 3 | Are retry attempts allowed, and do they reuse the same Order ID, create a new payment reference, or start a new order? | Yes — flow | Open |
| 4 | Does purchasing require login? How is visitor identity associated with order and wallet? | Yes — F-01 integration | Open |
| 5 | How does provider checkout return (deep link, browser, QR), and how is delayed verification polled/reconciled? | Yes — integration | Open |
| 6 | What recovery process/status applies when PAID is committed but e-ticket QR issuance later fails? | Yes — delivery | Open |
| 7 | Will proposed `UC-02A` be accepted in W0? Is 15-minute timer a separate technical Use Case? | Yes — traceability | Open |

---

## Completion checklist

- [x] Separate PNG, with conditional controls specified outside the representative mockup state.
- [x] Every visible/control element inventoried with source/validation.
- [x] Five states and success/error branches described.
- [x] All targets reference existing F-02 screens or expressly external team routes.
- [x] `AD-UC-02A` success/fail/invalid/duplicate/timeout logic and `FR-02-02`–`09` linked.
- [ ] Payment financial-resolution and team integration approvals pending.
