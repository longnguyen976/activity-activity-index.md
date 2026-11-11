# Screen Spec: S-F05-02 Partner Rental & Booking

| Field | Value |
|---|---|
| Screen ID | `S-F05-02` |
| Screen name | Partner Rental & Booking |
| Actor | F&B / Retail Partner (secondary actor: Staff & Admin, who maintain the space list and decide requests on S-F06-01) |
| Priority | Should |
| Feature | `F-05 Partner Platform & Event Management` |
| Activity diagrams | `AD-UC-06` |
| Mockup image | `img/S-F05-02.png` |
| Status | Draft |

Document Owner: Trần Hương Ly | Last Updated: 2026-11-07

## 1. Purpose

**Shown when:** A signed-in partner opens the Rentals tab to browse rental spaces, submit a rental request, or check the status of earlier requests.

**The user leaves this screen when:** The partner taps back to the Main Menu or switches to another tab.

## 2. Mockup

![S-F05-02](img/S-F05-02.png)

Layout overview:

- **Top bar:** Back button | title "Rental Spaces" | notification icon.
- **Segment control:** "Available Spaces" | "My Requests".
- **Tab 1, Available Spaces:** Scrollable list of space cards (name, location, size in m², price in VND, availability badge). Tapping a card opens the request form on the same screen.
- **Request form (inline panel):** Selected space summary, rental period, contact phone, notes, and the primary button "Submit Request".
- **Tab 2, My Requests:** List of the partner's requests with status (PENDING, APPROVED, REJECTED), reason for rejection, and "Edit & Resubmit" for rejected requests.
- **Overlay:** M-F05-03 Confirm Rental Request Dialog.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Page Title | Header | "Rental Spaces" (static) | Yes | None |
| 2 | Notification Icon | Icon button | Unread result count | No | Opens the notification list |
| 3 | Segment Switch | Tab toggle | "Available Spaces" \| "My Requests" | Yes | One tab active at a time |
| 4 | Space Card List | List | Rental spaces (FR-05-07) | Yes | Shows all spaces; Booked spaces are not requestable |
| 5 | Space Name | Text | `space.name` | Yes | None |
| 6 | Space Location | Text | `space.location` | Yes | None |
| 7 | Space Size | Text | `space.size_m2`, format "[n] m²" | Yes | None |
| 8 | Space Price | Text | `space.price`, format #,### VND | Yes | None |
| 9 | Availability Badge | Badge (icon + text) | `space.availability_status`: "Available" or "Booked" | Yes | Text label always shown, not colour only |
| 10 | Request Space Button | Primary button | "Request Space" | Yes | Disabled and labelled "Booked" when the space is not available |
| 11 | Rental Start Date | Date input | Label "Start date", DD/MM/YYYY | Yes | Not in the past |
| 12 | Rental End Date | Date input | Label "End date", DD/MM/YYYY | Yes | Not before the start date |
| 13 | Contact Phone | Text input | Label "Contact phone", format 0xxx xxx xxx | Yes | 10 digits starting with 0; error below the field |
| 14 | Notes Input | Text area | Label "Notes (optional)" | No | Maximum 500 characters |
| 15 | Submit Request Button | Primary button | "Submit Request" | Yes | Disabled until all required fields are valid |
| 16 | Request Card List | List | Partner requests (FR-05-08) | Yes | Newest first |
| 17 | Request Status Chip | Chip (icon + text) | `request.status`: "Pending", "Approved", "Rejected" | Yes | Text label always shown |
| 18 | Rejection Reason | Text | `request.reject_reason` | No | Shown only on rejected requests |
| 19 | Edit & Resubmit Button | Secondary button | "Edit & Resubmit" | No | Shown only on rejected requests |
| 20 | Empty Message | Banner | See states table | No | Shown when a list is empty |
| 21 | Confirm Button (in M-F05-03) | Primary button | "Confirm Request" | Yes | None |
| 22 | Cancel Button (in M-F05-03) | Secondary button | "Cancel" | Yes | Closes the dialog without sending |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | "Available Spaces" tab with space cards and availability badges | Partner opens the screen |
| Empty (no data) | Available Spaces: "No rental spaces are listed yet. Please check back soon." My Requests: "You have no requests yet. Choose a space to send your first request." with a "Browse Spaces" button | No spaces are listed, or the partner has no requests |
| Loading | Skeleton cards while lists load; "Submit Request" shows a spinner while the request is sent | Opening the screen, switching tab, or submitting |
| Error | Banner "We couldn't load the rental spaces. Please try again." with "Retry". If the space is no longer available: banner "This space is no longer available. Please choose another space." and the partner returns to the list | List request fails, or the space was booked by another approval (AD-UC-06 d1) |
| Success / confirmation | Toast "Request sent. The Operations team will review it." and the request appears as Pending in My Requests. When a result arrives: banner "Your request for [space] was approved." or "Your request for [space] was rejected. See the reason below." | Request created in PENDING state; Operations team decides |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Open the screen | Loads rental spaces and the partner's requests | stays |
| 2 | Segment Switch | Tap a tab | Shows the selected list | stays |
| 3 | Request Space Button | Tap | Checks that the space is still available (FR-05-07). If available, opens the request form with the space filled in. If not, shows the "no longer available" banner and returns to the list | stays |
| 4 | Submit Request Button | Tap | Validates the form; opens M-F05-03 | stays (modal) |
| 5 | Confirm Button (M-F05-03) | Tap "Confirm Request" | Creates the request in PENDING state and notifies the Operations team (FR-05-08); shows success toast | stays (My Requests tab) |
| 6 | Cancel Button (M-F05-03) | Tap "Cancel" | Closes the dialog; form data is kept | stays |
| 7 | Edit & Resubmit Button | Tap | Opens the request form pre-filled with the rejected request's data; a new PENDING request is created when submitted | stays |
| 8 | Notification Icon | Tap | Opens the list of request results | stays |
| 9 | Back button | Tap | Returns to the previous screen | Main Menu |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only signed-in partners can submit a request. | AD-UC-06 precondition |
| SR-002 | A space can be requested only when its availability status is Available. | FR-05-07; AD-UC-06 (d1) |
| SR-003 | A new request starts in PENDING state and the Operations team is notified. | FR-05-08 |
| SR-004 | A space is marked Booked only when Operations staff approve a request, not when a partner submits one. | AD-UC-06 (s6) |
| SR-005 | The partner is notified of the result; a rejected request shows the reason. | AD-UC-06 (s7, s9) |
| SR-006 | A rejected partner can edit and resubmit; each resubmission creates a new PENDING request. | AD-UC-06 (d3) |
| SR-007 | Prices are shown as #,### VND, dates as DD/MM/YYYY, phone as 0xxx xxx xxx. | UI Conventions |
| SR-008 | Creating or updating rental spaces is done by Staff on S-F06-01, not on this screen. | FR-05-07 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-05-07 | Displays space details (location, size, price) and availability status maintained by Staff |
| FR-05-08 | Lets the partner submit a rental request and see its status and result |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px.
- Mobile: single-column list; request form opens as a full-width panel under the selected card.
- Tablet / desktop web: two columns, space list on the left and request form or My Requests on the right; content width capped.
- Availability and request status are shown with text labels, never colour alone.
- Date and phone fields have visible labels; errors appear below the field; tap targets at least 24 by 24 CSS pixels (target 48 by 48 dp on mobile); body text contrast at least 4.5:1.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: FR-05-08 does not list the request form fields. Rental period, contact phone and notes are assumed.] | No | Open |
| 2 | [NEEDS CLARIFICATION: The UC-06 title mentions Sponsorship Deals, but no FR covers sponsorship. It is not on this screen.] | No | Open |
| 3 | [NEEDS CLARIFICATION: Can a rejected partner resubmit, and how many times? Drawn as no limit.] | No | Open |
| 4 | [NEEDS CLARIFICATION: Two partners may request the same space at the same time. Is a hold period needed?] | No | Open |
| 5 | [NEEDS CLARIFICATION: Staff screens for listing spaces and deciding requests are assumed to be in S-F06-01 (Hiệp). Please confirm.] | No | Open |
| 6 | [NEEDS CLARIFICATION: Priority of FR-05-07 and FR-05-08 is not stated in the files I have. Should is assumed.] | No | Open |

---

## Completion checklist

- [ ] The mockup is a separate cropped image file `img/S-F05-02.png`.
- [x] Every visible element appears in the element inventory (re-check after the mockup is drawn).
- [x] Every input element has a validation rule.
- [x] All five states are filled in.
- [x] Every navigation target exists in the Screen List.
- [x] Every element that displays data names its field.
- [x] Every linked FR exists in the Function List.
