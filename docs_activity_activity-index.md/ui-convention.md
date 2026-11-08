# UI Conventions: MySen Platform

## 1. Language & Tone

- **Interface Language:** English (Default for Release 1).
- **User Address:** Direct and friendly "you" / "your".
- **Button Labels:** Verb-first action descriptions in Title Case (e.g., "Submit Order", "Confirm Payment", "Redeem Reward", "Save Changes", "Get Directions", "Post a Photo"). Never use isolated generic words like "OK" or "Submit".

---

## 2. Standard Formats

| Item | Format | Example | Notes |
|---|---|---|---|
| Date | `DD/MM/YYYY` | 16/10/2026 | Standard date format |
| Time | `HH:mm` (24-hour) | 14:30 | 24-hour clock |
| Date and Time | `DD/MM/YYYY HH:mm` | 16/10/2026 14:30 | Timestamp format |
| Currency | `#,### VND` | 120,000 VND | Thousands separator is comma |
| Coin / Points | `#,### coins` | 1,250 coins | Platform loyalty currency |
| Numbers | Thousands separator is comma | 1,200 visitors | Standard numeric display |
| Phone Number | `0xxx xxx xxx` | 0912 345 678 | 10 digits starting with 0 |
| Personal Names | Full name field, Vietnamese diacritics supported | Nguyễn Hải Long | Displayed as typed |

---

## 3. Standard Component Rules

| Component | When to Use | Usage Rules & Notes |
|---|---|---|
| **Primary Button** | Main call-to-action per screen/panel | Maximum 1 per screen/tab; filled with primary brand color; disabled state clearly shaded. |
| **Secondary Button** | Alternative or supporting actions | Outlined or text style (e.g., "Cancel", "Share Achievement", "Try Again"). |
| **Text Input** | Data entry fields | Field label on top, inline validation error text displayed below field. |
| **Modal / Dialog** | Confirm or cancel critical single actions | Used for critical confirmations (e.g., "Log Out", "Confirm Redemption"); never used for main page navigation. |
| **Bottom Sheet** | Display contextual details or complex inputs | Slides up from bottom (e.g., Voucher Code display `M-F04-04`, Attraction Details `M-F03-01`, Create Post `M-F05-01`). |
| **Toast Notification** | Non-blocking brief feedback | Auto-dismisses after 3 seconds (e.g., "Profile saved.", "Report sent."). |
| **Banner** | Persistent situational information | Includes icon + text (e.g., Lock-out countdown, Offline alert, Streak ended alert). |
| **Segment Control / Tabs** | Switch sections within the same screen | Uses noun labels; active tab indicated by font weight and underline indicator, not color alone. |
| **Badge / Status Chip** | Display state or counter | Always includes text label + icon (e.g., "Open", "Closed", "Active", "Redeemed", "Booked"). |
| **Map Marker** | Point of interest on 2.5D map | Accessible text alternative provided for screen readers. |

---

## 4. Message & Feedback Patterns

| Situation | Design Pattern | Standard Example |
|---|---|---|
| **Validation Error** | Explicit explanation + how to fix | *"Enter a valid phone number or email address."* |
| **System / Network Error** | Actionable cause + recovery button | *"We couldn't load your profile. Check your connection and try again."* |
| **Empty State** | Clear reason + primary call-to-action | *"The gallery is empty. Be the first to share a photo!"* |
| **Success Confirmation** | Brief positive confirmation | *"Voucher redeemed! You can find it in My Vouchers."* |
| **Permission Denied** | Explanation + fallback guidance | *"Camera access is required to scan QR codes. You can also enter the code manually."* |

---

## 5. Accessibility & Responsive Guidelines (WCAG 2.2)

- **Mobile Baseline:** Minimum target width 360px. Single-column responsive layout.
- **Touch Target Size:** Minimum 24x24 CSS px (recommended 48x48 dp on mobile touchscreens).
- **Color Contrast:** Minimum 4.5:1 contrast ratio for body text against background.
- **Color Independence:** Status and state information must never be conveyed by color alone (always pair color with icons, text badges, or labels).
- **Screen Reader Support:** All images/icons provide alt text; interactive components use proper ARIA labels and focus trap inside modals.
