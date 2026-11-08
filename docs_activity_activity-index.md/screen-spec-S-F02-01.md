# Screen Spec: S-F02-01 E-Ticket Catalogue

| Field | Value |
|---|---|
| Screen ID | `S-F02-01` |
| Screen name | E-Ticket Catalogue |
| Actor | Visitor / GenZ Customer |
| Priority | Must |
| Feature | `F-02` E-Ticketing & Commerce |
| Activity diagrams | `AD-UC-02A.md` (purchase; proposed `UC-02A` from W0 `UC-02`) |
| Mockup image | `img/S-F02-01.png` |
| Owner | Nguyễn Phạm Nguyệt Minh |
| Status | Draft specification with complete template; pending team source/policy approval |

## 1. Purpose

**Shown when:** A visitor opens the ticketing feature to browse available e-tickets, or returns from Checkout or Ticket Wallet to begin a selection.

**The user leaves this screen when:** A valid ticket type and positive quantity are selected and the visitor taps **Continue to Checkout**, reaching `S-F02-02`; or Back returns to the team's shared entry point.

**Source trace:** `AD-UC-02A` actor nodes `a1` (browse), `a2` (select); system node `s1` (display ticket types/prices, `FR-02-01`) and the subsequent order-validation requirement `FR-02-02`. **No order is persisted on this screen.** Authentication before browsing is not an approved precondition.

## 2. Mockup

![S-F02-01 E-Ticket Catalogue](img/S-F02-01.png)

> Mobile low-fidelity wireframe. Ticket names, prices, validity and quantity illustrations are placeholders, not real Đầm Sen catalogue data; the figure is a layout specification, not a deployed UI.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | `CatalogueHeader` | Header + Back button | Static “Tickets”; returns to shared entry route | Yes | Navigation target defined by team app shell |
| 2 | `CatalogueFeatureLabel` | Text | “F-02 · E-Ticketing & Commerce” in mockup (design annotation; remove from production if team wants) | No | Not business data |
| 3 | `CatalogueIntro` | Heading / helper text | “Choose your tickets” and instructions | Yes | Static UI copy |
| 4 | `TicketList` | List / repeated card | W0 ILF `Ticket catalogue` via `FR-02-01` | Yes | Render currently sellable entries; unavailable may appear disabled for reference, depending on team decision |
| 5 | `TicketName` | Text per item | `Ticket catalogue`: ticket type/name | Yes | Cannot be empty for display |
| 6 | `TicketPrice` | Text per item | `Ticket catalogue`: current price | Yes | Read-only, VND; final price validated on server at order creation |
| 7 | `TicketConditions` | Text per item | `Ticket catalogue`: current conditions / validity | Yes | No fabricated validity window |
| 8 | `AvailabilityLabel` | Badge / text | `Ticket catalogue`: availability/sellability | Yes | Clearly show “Available” or “Unavailable”, not colour alone |
| 9 | `QuantitySelector` | Increment / decrement input | Local selection `ticket_type_id`, `quantity` | Yes | Integer ≥ 0; positive selection required to continue; maximum from **unapproved** product settings; disabled for unavailable product |
| 10 | `SelectedTicketsSummary` | Summary panel | Local selection count and estimated subtotal | Yes | Derived for display only; authoritative amount recalculated in `FR-02-02` |
| 11 | `ContinueCheckoutButton` | Primary button | “Continue to Checkout” | Yes | Enable only when ≥1 valid, available product has quantity > 0 |
| 12 | `SubtotalHelperText` | Text | “Final amount confirmed at checkout.” | Yes | Must not imply order already placed |
| 13 | `CatalogueStatusBanner` | Banner / state slot | Error, loading, empty, stale-product and recovery messages | Conditional | Status sourced from `FR-02-01` response |
| 14 | `BottomNavigation` | Shared app navigation | MySen / Tickets / Account (illustrative app shell) | Conditional | F-00/F-01 routes owned by shared navigation team; do not create F-02 IDs |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Sellable ticket cards, current price, conditions, quantity controls; summary updates with selection. | Catalogue response successfully loaded. |
| Empty (no data) | “No tickets are available right now. Check back later.” Continue action disabled. | No sellable products returned. |
| Loading | Skeleton ticket cards, selection and Continue disabled; accessible “Loading tickets” announcement. | Catalogue request in progress. |
| Error | “We couldn't load tickets. Try again.” plus **Try Again**; after stale selection, prompt to review updated availability. | Network/backend error or conflicting catalogue state. |
| Success / confirmation | Visitor sees updated selected count and proceeds to Checkout. This is **selection success**, not paid success. | Tap Continue with valid positive selection. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Ticket feature entry | Open Tickets | Fetch current ticket catalogue; show Loading, then Default / Empty / Error. | `S-F02-01` |
| 2 | `QuantitySelector` | Tap + / − | Update local nonnegative integer within configured bound; update summary. | stays |
| 3 | `TicketConditions` | Tap for more details where truncated | Expand provided terms inline; no new route. | stays |
| 4 | `ContinueCheckoutButton` | Tap with valid selection | Validate selected ticket IDs/quantities, send selection to Checkout; do not create order yet. | `S-F02-02` |
| 5 | `ContinueCheckoutButton` | Tap with none/invalid/unavailable | Block progression; show specific validation message. | stays |
| 6 | `CatalogueStatusBanner` | Tap Try Again | Re-fetch from `Ticket catalogue`, then revalidate any retained selection. | stays |
| 7 | `CatalogueHeader` | Tap Back | Leave ticketing to prior shared screen, without creating an order. | stays (external team route) |
| 8 | `BottomNavigation` | Tap shared navigation item | Route through app shell according to team Screen List; no new F-02 screen invented. | stays (shared route TBD) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| `SR-F02-01-01` | Show ticket types currently available for purchase, with current prices and conditions. | `FR-02-01`; `AD-UC-02A s1` |
| `SR-F02-01-02` | A selected ticket must have a valid type and positive integer quantity before Checkout progression. | `FR-02-02`; `AD-UC-02A a2` |
| `SR-F02-01-03` | An invalid/unavailable selection must not produce an order; the server revalidates at Checkout. | UC-02 Sequence guard; `FR-02-02` |
| `SR-F02-01-04` | Price and subtotal shown prior to order creation are indicative and never authoritative payable amounts. | `FR-02-02` requires system-computed total |
| `SR-F02-01-05` | Quantity ceiling, guest eligibility and discount-code availability cannot be assumed without approved source rules. | `AD-UC-02A` §6 open questions |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| `FR-02-01` | Fetches and shows the current sellable catalogue with price and purchasing conditions. |
| `FR-02-02` | Collects ticket selection and quantity for Checkout's eventual server-side order creation; **order is not created here**. |

## 8. Responsive and accessibility notes

- Smallest supported width: **proposed 360 CSS px**; group design-system baseline remains unconfirmed.
- Cards stack vertically, price and availability remain readable; quantity button labels include ticket names for screen readers.
- Do not communicate availability only by green/grey. Show explicit text states.
- Body text ≥4.5:1 contrast; targets ≥24×24 CSS px per supplied Lesson 8 UI template; keyboard focus order follows the catalogue layout.
- Ticket dates/prices are read from live catalogue records. A sample mockup value is not valid system input.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Is sign-in needed before browsing, before creating an order, or only before opening a personal Ticket Wallet? | Yes — F-01 integration | Open |
| 2 | What are the actual ticket types, price records, validity conditions and maximum per-order quantities? | Yes — real catalogue | Open |
| 3 | Does the purchase flow include a promo code? The UC-02 sequence has an optional mention, but `AD-UC-02A` notes no confirmed W1 FR. | No — release scope | Open |
| 4 | Is the group UI shell/bottom navigation exactly as illustrated? Which global routes should Back target? | No — team design | Open |
| 5 | Will proposed `UC-02A` be baselined in W0, or remain the purchase phase of parent `UC-02`? | Yes — traceability | Open |

---

## Completion checklist

- [x] Separate cropped PNG named with the Screen ID.
- [x] Visible elements (including shared shell) covered in inventory.
- [x] Input validation specified.
- [x] All five states and interactions covered.
- [x] In-F-02 navigation targets exist in assigned Screen List.
- [x] Actual `FR-02-01/02` codes, W0 data and AD node mapping documented.
- [ ] Group policy/sign-in/design-system approval pending.
