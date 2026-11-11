# Screen Spec: S-F02-03 Ticket Wallet

| Field | Value |
|---|---|
| Screen ID | `S-F02-03` |
| Screen name | Ticket Wallet |
| Actor | Visitor / GenZ Customer |
| Priority | Must for e-tickets; Should for refund interactions |
| Feature | `F-02` E-Ticketing & Commerce |
| Activity diagrams | `AD-UC-02A.md` (issued QR ticket; proposed `UC-02A`) and `AD-UC-02B.md` (later independent refund; proposed `UC-02B`) |
| Mockup image | `img/S-F02-03.png` |
| Owner | Nguyễn Phạm Nguyệt Minh |
| Status | Draft specification with complete template; refund policy/approval unconfirmed |

## 1. Purpose

**Shown when:** The visitor opens **My Tickets** from a verified successful Checkout or directly from the ticket shortcut to view owned ticket(s). A visitor with a previously PAID order may separately request cancellation/refund via a conditional panel (**Should** feature).

**The user leaves this screen when:** They tap **Browse Tickets** → `S-F02-01`, or Back returns to the shared app entry point. Refresh, display QR, submit refund and see refund outcome all remain on the same screen.

**Source trace:** `AD-UC-02A` `a5/s6/s7`: ticket issued only after PAID and QR generated under `FR-02-08/09`. `AD-UC-02B` `a1/a2/a3`, system `s1–s10`, decisions `d1/d2`: ownership + policy before refund; original payment provider; `REFUND_PENDING` until final provider result; invalidate affected unused QR **only after verified SUCCESS**. This is **not** the F-04 loyalty coin or F&B Voucher Wallet.

## 2. Mockup

![S-F02-03 Ticket Wallet](img/S-F02-03.png)

> The PNG is an illustrative single-screen mockup: the QR-like graphic is visibly labelled **DEMO / NOT VALID**, contains no ticket credential, and cannot be used for entrance. The refund panel shown is **conditional** and represents an eligible case, not a universally approved refund right.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | `WalletHeader` | Header / Back button | “My Tickets”, Back | Yes | Shared app route TBD |
| 2 | `WalletFeatureLabel` | Text | “F-02 · Ticket Wallet” (mockup annotation) | No | Static |
| 3 | `WalletHeading` | Heading | “Your e-tickets” | Yes | Static |
| 4 | `TicketCollection` | List / cards | W0 ILF `Order, ticket & booking`; only visitor-owned ticket/order data | Yes | Check authenticated identity / ownership (F-01 dependency) |
| 5 | `TicketName` | Text | Issued ticket's catalogue type/name | For ticket | Must match server-issued item |
| 6 | `TicketStatus` | Text / badge | Derived from ticket validity/issue state, not Payment URL; e.g. Issued / Pending / Refunded | For ticket/order | “Issued” only when ticket record actually exists and is valid |
| 7 | `TicketIdText` | Text | Server-issued `ticket_id` | For issued ticket | Must not fabricate; copyable/accessibly readable |
| 8 | `OrderReference` | Text | Source `order_id` | For issued ticket | Ticket/order/customer association verified |
| 9 | `TicketValidity` | Text | Configured ticket validity and date/time | Where source provides | No invented entry date, used/expiry policy |
| 10 | `TicketQrCode` | Image | Server-generated protected credential (`FR-02-09`); mockup uses non-scannable illustration | Only for issued, currently valid ticket | No QR if payment not confirmed, ticket not issued, or invalidated after confirmed refund |
| 11 | `TicketQrHelpText` | Text | “Show this QR at the entrance”; text-based ticket reference alternative | With live QR | Avoid exposing QR secret in alt text |
| 12 | `BrowseTicketsButton` | Secondary button | “Browse Tickets” | Yes | None |
| 13 | `WalletStatusBanner` | Banner / state slot | Empty/loading/error, ticket processing, already refunded, QR invalidated | Conditional | Fetch authoritative current server state |
| 14 | `RefundPanelHeading` | Expandable panel header | “Cancellation & refund (if eligible)” | Should; only where approved | Do not assume all issued tickets are refundable |
| 15 | `RefundPolicyHint` | Text | Backend-configured refund rule/eligibility result or clear “Policy pending” | Should | No made-up deadlines/approval rights |
| 16 | `RefundReasonInput` | Input in expanded panel | Customer reason text if configured by final refund policy | Should/conditional | Requiredness and max length **[NEEDS CLARIFICATION]**; apply server-side validation |
| 17 | `RequestRefundButton` | Secondary / in-panel button | “Request Refund” | Should; when owned paid unused ticket passes configured eligibility | Reject duplicate request; backend may recheck policy |
| 18 | `RefundConfirmModal` | Modal | Confirm submission of refund request, if approved by team | Conditional | Modal is **not** a separate Screen ID |
| 19 | `RefundStatusText` | Text / banner | Rejected reason / `REFUND_PENDING` / `REFUNDED` / recoverable failed or pending status | If request exists | Only call completed after verified gateway SUCCESS; no premature QR invalidation |
| 20 | `RefundRefreshButton` | Secondary button | “Check Refund Status” (if delayed) | Conditional | Read-only status check; don't reinitiate refund |
| 21 | `QrExpandOverlay` | Overlay | Larger authorized QR, same ticket credential | Conditional | Must not show after confirmed invalidation |
| 22 | `BottomNavigation` | Shared app navigation | MySen / Tickets / Account (illustrative shell) | Conditional | F-00/F-01 route owner to confirm |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Owned issued-ticket cards with Ticket ID, Order ID, validity and valid QR. Conditional “Cancellation & refund” option for eligible **PAID unused** ticket. | Server returns authorized issued ticket and refund eligibility. |
| Empty (no data) | “You have no tickets yet.” and **Browse Tickets**; no live QR. | No owned/issued records to show. |
| Loading | Ticket skeleton; QR slot displays *no scannable credential*; status announcement. | Wallet/detail/refund-status request pending. |
| Error | Failed load/ownership check, ticket still processing, invalidated ticket, ineligible refund, provider refund failure, or `REFUND_PENDING`; show specific text and recovery/status action. These are distinct business conditions, not a single `FAILED` ticket enum. | Backend returns error, no final refund result or failing eligibility; `AD-UC-02B d1/d2`. |
| Success / confirmation | **Ticket success:** issued QR available after verified payment; **refund request accepted:** show `REFUND_PENDING`, **not** “refunded”; **refund success:** only after original provider's verified SUCCESS show REFUNDED and remove/invalidate QR. | `FR-02-08/09`, then optional `FR-02-10/11/12`. |

### Refund substates inside S-F02-03 (not additional Screen IDs)

| Refund condition | Visible outcome | QR policy |
|---|---|---|
| Ineligible / request rejected | Reason from configured rule; no refund initiated | Maintain previously valid ticket status as provided by backend |
| Eligible request submitted | Pending status, original-provider operation | Do not claim invalidated solely on submission |
| `REFUND_PENDING` | “Your refund is being processed” and Check Status | Do **not** mark as successfully refunded |
| Verified provider `SUCCESS` | `REFUNDED`, show confirmation | Invalidate all **affected unused** QR credentials (as specified by W1) |
| Verified provider `FAILED` or recoverable failure | Recoverable failed/pending status with approved support path | Do not claim REFUNDED or successful invalidation |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | My Tickets entry | Open Wallet | Check visitor ownership/session, query owned orders and issued ticket records. | `S-F02-03` |
| 2 | `TicketCollection` | Select a ticket | Read authoritative ticket/order status; show QR only when issued and valid. | stays |
| 3 | `TicketQrCode` | Present at entrance / tap enlarge | Optionally open same authorized QR in overlay; gate validation policy is external/TBD. | stays |
| 4 | `WalletStatusBanner` | Tap Try Again / Check Status | Re-fetch state; never issue duplicate ticket from a client refresh. | stays |
| 5 | `BrowseTicketsButton` | Tap Browse Tickets | Open catalogue for new selection. | `S-F02-01` |
| 6 | `RefundPanelHeading` | Expand for eligible ticket | Fetch/show current refund eligibility and confirmed business-policy disclosure. | stays |
| 7 | `RefundReasonInput` | Enter reason if required | Validate field only if configured; do not invent mandatory length. | stays |
| 8 | `RequestRefundButton` | Tap Request Refund (Should) | Submit request tied to **owned PAID unused** ticket; server checks eligibility/approval under `FR-02-10`. | stays |
| 9 | `RefundConfirmModal` | Confirm / cancel submission (if enabled) | Confirm submits; cancel performs no refund operation. | stays |
| 10 | Provider refund event (no user action) | Original MoMo/VNPay returns outcome | Keep REFUND_PENDING until result; on verified success mark refund/order REFUNDED and invalidate affected unused QR; failed case keeps recoverable state. | stays |
| 11 | `RefundRefreshButton` | Tap Check Refund Status | Read latest recorded provider outcome; never resubmit the original refund merely to refresh. | stays |
| 12 | `WalletHeader` | Tap Back | Return to shared app entry; Wallet state remains backend-owned. | stays (shared route TBD) |
| 13 | `BottomNavigation` | Tap shared app navigation | Follow team-owned app navigation policy. | stays (shared route TBD) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| `SR-F02-03-01` | Show only ticket/order records belonging to authorized visitor; QR must be derived from actual issued ticket, never a client-generated mock code. | `FR-02-08/09`; UC-02 sequence retrieval |
| `SR-F02-03-02` | Ticket may be displayed as “Issued” with QR only after verified payment, successful record creation and protected credential generation. | `FR-02-08/09`; `AD-UC-02A s6/s7` |
| `SR-F02-03-03` | A previously PAID order may initiate `UC-02B` refund later; it is not an automatic post-purchase step. | `AD-UC-02B` trigger and §1 |
| `SR-F02-03-04` | Only an owned PAID unused ticket enters refund eligibility review; policy window/approval constraints remain configured, not invented. | `FR-02-10`; `AD-UC-02B s2/d1` |
| `SR-F02-03-05` | Eligible or approved refund is sent to the original provider using the original transaction reference, marked REFUND_PENDING. | `FR-02-11`; `AD-UC-02B s5` |
| `SR-F02-03-06` | Do **not** mark refund/order REFUNDED until provider confirms verified SUCCESS. | `FR-02-12`; `AD-UC-02B d2/s6` |
| `SR-F02-03-07` | Only a confirmed successful refund invalidates **affected unused** ticket QR; failed/pending refund must remain distinguishable and recoverable. | `FR-02-12`; `AD-UC-02B s7/s9/s10` |
| `SR-F02-03-08` | If PAID but QR issuance has not completed, display “Ticket processing”; do not reveal a valid-looking placeholder or create duplicate ticket. | `FR-02-08/09`; UC-02 sequence issuance exception |
| `SR-F02-03-09` | This screen is an **e-ticket** wallet, not an F-04 **coin/reward** or **F&B voucher** wallet. | W0 feature ownership; `AD-UC-02C` cross-feature boundary |
| `SR-F02-03-10` | Partial refund, refund of used tickets and manual review roles are deliberately unimplemented in UI pending policy. | `AD-UC-02B` §6 open questions |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| `FR-02-08` | Displays owned ticket records issued following verified PAID order; shows processing while issuance pending. |
| `FR-02-09` | Shows server-backed protected QR and validity details only for issued/valid ticket; hides after confirmed invalidation. |
| `FR-02-10` | **Should:** Conditional panel for cancellation/refund, eligibility, reason and request outcome. |
| `FR-02-11` | **Should:** Original-provider `REFUND_PENDING` state visible without claiming money returned. |
| `FR-02-12` | **Should:** Shows verified REFUNDED and affected-QR invalidation or recoverable failed result. |
| `FR-02-04/05/06` | Read-only effect of PAID, unsuccessful provider payment and EXPIRED order where relevant; not reimplemented by Wallet. |

## 8. Responsive and accessibility notes

- Smallest supported width: proposed **360 CSS px**, subject to group approval; stack ticket card, QR, refund panel and action controls vertically.
- The QR is square and paired with Ticket ID, status and validity in readable text; actual QR credential is **never** published in image alt text.
- Announce asynchronous refund updates. Ensure modal focus trapping/return to `RequestRefundButton` if a confirm modal is used.
- Body contrast ≥4.5:1, target ≥24×24 CSS px; status not conveyed by colour only, as required by the lesson template.
- Prefer retaining a readable full Ticket ID and Order ID even if the QR cannot be rendered. Ticket entry validity at the physical gate requires a separate confirmed operational rule.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | What are actual ticket validity dates, gate/QR scan acceptance and used-ticket status policy? | Yes — operation | Open |
| 2 | What refund window, unused/partially used requirement, reason, approval actor and review status are approved? | Yes — `UC-02B` | Open |
| 3 | Is the refund panel formally accepted inside `S-F02-03`, or does the group need a fourth F-02 screen and revised assignment? | Yes — screen scope | Open; inline conditional panel used |
| 4 | How will a later gateway SUCCESS/FAILED be applied when refund remains REFUND_PENDING? Is refund callback secured/idempotent? | Yes — money correctness | Open |
| 5 | If confirmed refund succeeds but QR invalidation fails, what reconciliation/retry path guarantees ticket cannot be used? | Yes — consistency | Open |
| 6 | Are partial refunds per ticket item allowed, or only full-order refunds? | Yes — data model | Open |
| 7 | Is sign-in mandatory to view tickets or can a guest receive/retrieve purchased tickets? | Yes — F-01 integration | Open |
| 8 | Does the business want refunded/inactive tickets visible in historical list or hidden? | No — UI decision | Open |
| 9 | Will `UC-02B` be approved in W0 or documented as a later use-case phase of parent UC-02? | Yes — traceability | Open |

---

## Completion checklist

- [x] One PNG with visually distinct ticket and conditional refund zone; no real QR/token in mockup.
- [x] Every visible and conditional UI component inventoried.
- [x] Five screen states plus meaningful refund subconditions and navigation.
- [x] `AD-UC-02A` ticket and `AD-UC-02B` refund branch mapping complete.
- [x] All F-02 screen navigation targets reference assigned IDs; conditional refund stays within Wallet.
- [ ] Human review of refund policy / financial callback and UI allocation pending.
