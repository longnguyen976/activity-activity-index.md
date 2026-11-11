# Screen Spec: S-F06-01 Admin Dashboard

| Field | Value |
|---|---|
| Screen ID | `S-F06-01` |
| Screen name | Admin Dashboard |
| Actor | Marketing & Sales Dept; IT & Technical Dept |
| Priority | Must (Overview tab: Could; Push Notifications tab: Should. See SR-011) |
| Feature | `F-06 Admin Operations & Analytics` |
| Activity diagrams | `AD-UC-08`, `AD-UC-09` (file not provided), `AD-UC-10` |
| Mockup image | `img/S-F06-01.png` |
| Status | Draft |

## 1. Purpose

**Shown when:** a signed-in staff member opens the admin console after signing in. The tabs shown depend on the role.

**The user leaves this screen when:** the user confirms Log Out (goes to S-F00-01) or the session expires (goes to S-F00-01). Switching tabs does not leave the screen.

## 2. Mockup

![S-F06-01](img/S-F06-01.png)

*Image placeholder: export the design frame to `img/S-F06-01.png` (Overview tab). The other tabs and the campaign form are saved as `img/S-F06-01-campaigns.png`, `-push.png`, `-roles.png` and `-campaign-form.png`.*

Layout, top to bottom: the top bar (title, user name and role, Log Out), the section tabs, then the content of the selected tab.

- **Overview:** the filter bar (date range, park zone), three figure cards, the visitors-by-hour chart, the visitors-by-zone list, the redemptions-by-campaign list.
- **Campaigns:** the New Campaign button and the campaign list. The campaign form opens in a side panel (desktop) or a full-width panel (mobile).
- **Push Notifications:** the compose form (title, message, audience, send time) and the send button.
- **Staff & Roles:** the staff list with a role selector and an Assign Role button on each row.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Admin Top Bar | Header | Title "MySen Admin", the signed-in user's name and role. Source: User account & profile and Role & permission (FR-01-08) | Yes | None |
| 2 | Log Out Button | Secondary button | "Log Out" (FR-01-06) | n/a | Opens M-F01-01 |
| 3 | Section Tabs | List (tabs) | "Overview", "Campaigns", "Push Notifications", "Staff & Roles". A tab is shown only if the role allows it (FR-01-08) | Yes | At least one tab is visible. The first visible tab opens by default [NEEDS CLARIFICATION: Q-03] |
| 4 | Date Range Picker | Input (date range) | "From" and "To", `DD/MM/YYYY`. Default: the last 7 days [NEEDS CLARIFICATION: Q-02] | Yes | From is not after To. Neither is later than today. Error: "Choose an end date on or after the start date." |
| 5 | Park Zone Filter | Input (dropdown) | "All zones" plus the zones of the park. Source: Park map & points of interest (FR-06-04) | No | Default "All zones" |
| 6 | Total Visitors Card | Text (card) | Number of visitors in the range and zone, format `#,###`. Source: User-behaviour event, Order, ticket & booking (FR-06-04, FR-06-05) | n/a | None |
| 7 | Ticket Revenue Card | Text (card) | Ticket revenue in the range, format `#,### VND`. Source: Order, ticket & booking (FR-06-04) | n/a | None |
| 8 | Promotion Redemptions Card | Text (card) | Number of promotion codes redeemed in the range. Source: Marketing campaign & promotion code (FR-06-04) | n/a | None |
| 9 | Visitors by Hour Chart | Image (chart) | Visitors per hour from 00:00 to 23:00, one line per park zone, or one line for the chosen zone. A "View as table" link shows the same numbers. Source: User-behaviour event (FR-06-04) | n/a | None |
| 10 | Visitors by Zone List | List | Zone name and visitor count, highest first. Source: User-behaviour event (FR-06-04) | n/a | None |
| 11 | Redemptions by Campaign List | List | Campaign name and codes redeemed in the range. Source: Marketing campaign & promotion code (FR-06-04) | n/a | None |
| 12 | Campaign List | List | Per row: name, period `DD/MM/YYYY HH:mm` to `DD/MM/YYYY HH:mm`, audience, discount rule, codes redeemed of codes generated, status "Scheduled", "Active" or "Ended" (icon and text). Source: Marketing campaign & promotion code (FR-06-01) | n/a | None |
| 13 | New Campaign Button | Primary button | "New Campaign". Hidden while the campaign form is open | n/a | Opens the form (#14 to #21) |
| 14 | Campaign Name | Text input | Label "Campaign name" (FR-06-01) | Yes | 1 to 100 characters [NEEDS CLARIFICATION: Q-04]. Error: "Enter a campaign name." |
| 15 | Campaign Start | Input (date and time) | Label "Starts", `DD/MM/YYYY HH:mm` | Yes | Not in the past when creating. Error: "Choose a start time that is not in the past." |
| 16 | Campaign End | Input (date and time) | Label "Ends", `DD/MM/YYYY HH:mm` | Yes | After the start. Error: "End time must be after the start time. Choose a later end time." |
| 17 | Campaign Audience | Input (dropdown) | Label "Audience". Simple criteria such as "All visitors" or an age range [NEEDS CLARIFICATION: Q-04] | Yes | One value must be chosen. Error: "Choose an audience." |
| 18 | Discount Rule | Input (dropdown and number) | Label "Discount". Type "Percentage" or "Fixed amount (VND)" and a value (FR-06-01) | No | Percentage: whole number 1 to 100. Fixed amount: whole number above 0, shown as `#,### VND`. Error: "Enter a discount between 1% and 100%." |
| 19 | Code Batch Size | Text input (number) | Label "Number of promotion codes" | Yes | Whole number of at least 1, up to a maximum [NEEDS CLARIFICATION: Q-04]. Error: "Enter a whole number of 1 or more." |
| 20 | Save Campaign Button | Primary button | "Save Campaign" | n/a | Enabled when #14 to #17 and #19 are filled |
| 21 | Cancel Form Button | Secondary button | "Cancel". Closes the form and discards what was typed | n/a | None |
| 22 | Campaign Row Actions | Button (text) | "Edit" on a Scheduled or Active campaign. "End Campaign" on a Scheduled or Active campaign | n/a | An Ended campaign has no actions |
| 23 | Push Title | Text input | Label "Title" (FR-06-02) | Yes | 1 to 50 characters [NEEDS CLARIFICATION: Q-04]. Error: "Enter a title." |
| 24 | Push Message | Text input (multi-line) | Label "Message" | Yes | 1 to 150 characters [NEEDS CLARIFICATION: Q-04]. Error: "Enter a message." |
| 25 | Push Audience | Input (dropdown) | Label "Send to". Same criteria as #17. Only devices that allowed notifications receive it | Yes | One value must be chosen. Error: "Choose an audience." |
| 26 | Send Time | Input (radio and date and time) | "Send now" or "Schedule for later" with a date and time `DD/MM/YYYY HH:mm` | Yes | A scheduled time must be later than now. Error: "Choose a time in the future." |
| 27 | Send Notification Button | Primary button | "Send Notification", or "Schedule Notification" when scheduled | n/a | Enabled when #23 to #26 are valid. Opens M-F06-02 |
| 28 | Staff List | List | Per row: name, account (phone or email), current role. Source: User account & profile, Role & permission (FR-06-09) | n/a | None |
| 29 | Role Selector | Input (dropdown) | The fixed roles of the platform, for example "Marketing Manager" and "IT Administrator" [NEEDS CLARIFICATION: Q-03] | Yes | One role per staff account |
| 30 | Assign Role Button | Primary button | "Assign Role" on each row | n/a | Enabled when the chosen role differs from the current role |
| 31 | Permission Error | Banner | "You don't have permission to manage roles." (HTTP 403, FR-01-08) | No | Shown only when the server refuses the request |
| 32 | Success Toast | Toast | For example "Campaign saved.", "Notification sent.", "Role updated." | No | Disappears by itself after 3 seconds |
| 33 | Load Error | Banner | "We couldn't load this section. Check your connection and try again." with a "Try Again" button | No | Shown only when a load fails |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | The first tab the role allows. Overview shows the last 7 days for all zones with the three cards, the chart and the two lists filled. Campaigns shows the campaign list. Push Notifications shows an empty compose form. Staff & Roles shows the staff list | The screen opens after sign-in |
| Empty (no data) | Overview: "No visits or sales were recorded for these dates. Try a wider date range." Campaigns: "No campaigns yet. Create your first campaign to generate promotion codes." with New Campaign. Staff & Roles: "No staff accounts found." Push Notifications: not applicable, because it is a form | The range or the list has no records |
| Loading | Grey placeholders for the cards, chart and lists. The button that was tapped shows a spinner and is disabled | A tab opens, a filter changes, or the user taps a save or send button |
| Error | Load Error (#33) with Try Again. Inline field errors in the forms. Permission Error (#31) on Staff & Roles. After a failed save: "We couldn't save your changes. Your changes are still here. Try again." If the session expired, the user goes to S-F00-01 with the message "Your session has ended. Sign in again." | A call fails, a field is invalid, the role is not allowed (HTTP 403), or the session is no longer valid |
| Success / confirmation | A toast: "Campaign saved. Status: Scheduled." / "Campaign ended." / "Notification sent." / "Notification scheduled for 31/10/2026 18:00." / "Role updated. The new permissions apply from the next request." | The server accepts the action |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Opens after sign-in | Checks the role, shows the allowed tabs and loads the first one (FR-01-08) | stays |
| 2 | Date Range Picker (#4), Park Zone Filter (#5) | Changes a value | Adds up the visitors, ticket revenue and redemptions for the new range and zone (FR-06-04) | stays |
| 3 | Section Tabs (#3) | Taps a tab | Loads the data of that tab | stays |
| 4 | New Campaign Button (#13) | Taps | Opens an empty campaign form | stays |
| 5 | Save Campaign Button (#20) | Taps with valid data | Stores the campaign, generates its promotion codes, adds an audit record, sets the status to Scheduled. It becomes Active at the start time (FR-06-01, FR-06-10) | stays (list updated) |
| 6 | Save Campaign Button (#20) | Taps with invalid data (for example an end before the start) | Shows the errors, keeps what was typed, saves nothing, generates no codes and adds no audit record | stays |
| 7 | Campaign Row Actions (#22) | Taps "Edit", then Save Campaign | Opens the form with the saved values. A save follows the same path as a new campaign and is also audited (FR-06-01, FR-06-10) | stays |
| 8 | Campaign Row Actions (#22) | Taps "End Campaign" | Opens M-F06-01 | stays |
| 9 | M-F06-01 | Confirms "End Campaign" | Sets the status to Ended and adds an audit record (FR-06-01, FR-06-10) | stays |
| 10 | Send Notification Button (#27) | Taps with valid data | Opens M-F06-02 | stays |
| 11 | M-F06-02 | Confirms "Send" | Finds the devices that allowed notifications. Sends at once, or queues the send for the chosen time, and records the delivery status per device (FR-06-02) | stays (form cleared) |
| 12 | Assign Role Button (#30) | Taps | Checks that the caller may manage roles, stores the role, adds an audit record, and applies the permissions from the next request (FR-01-08, FR-06-09, FR-06-10) | stays |
| 13 | Assign Role Button (#30) | Taps without permission | The server answers HTTP 403. Shows #31 and changes nothing | stays |
| 14 | Log Out Button (#2) | Taps | Opens M-F01-01 | stays |
| 15 | M-F01-01 | Confirms "Log Out" | Ends the session on this device and revokes the current refresh token (FR-01-06) | `S-F00-01` |
| 16 | Screen | The access token expires and the silent refresh fails | Ends the session and shows the sign-in screen (FR-01-10) | `S-F00-01` |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Tabs by role: Overview, Campaigns and Push Notifications for Marketing & Sales Dept; Staff & Roles for IT & Technical Dept. | FR-01-08, UC-08, UC-09, UC-10 [Q-03] |
| SR-002 | Every request is checked against the role. A request outside the role is refused with an authorisation error (HTTP 403). | FR-01-08 |
| SR-003 | A campaign must end after it starts. If not, nothing is saved, no codes are generated and no audit record is added. | FR-06-01, AD-UC-08 BT-08-01 |
| SR-004 | A saved campaign is Scheduled and turns Active by itself at its start time. | FR-06-01 |
| SR-005 | Editing and ending a campaign use the same save path as creating one, and are audited. | FR-06-01, FR-06-10, AD-UC-08 A-03 |
| SR-006 | A promotion code's expiry and usage limit are enforced at checkout, never on this screen. The screen only shows counts. | FR-06-03 |
| SR-007 | A push reaches only devices that allowed notifications. A scheduled time must be in the future. | FR-06-02 |
| SR-008 | A role assigned here applies from the next request of that staff account. | FR-06-09 |
| SR-009 | Creating, changing or ending a campaign and assigning a role are each recorded in the append-only audit log. There is no audit log screen in release 1. | FR-06-10, Scope Table 3 |
| SR-010 | The Overview shows totals only, never individual visitors. Behaviour events are stored with a pseudonymised user ID. | FR-06-05 |
| SR-011 | Build order by effective priority: Campaigns and Staff & Roles are Must, Push Notifications is Should, Overview is Could. If capacity runs short, the Overview is built last. | Prioritization sheet |
| SR-012 | Dates, times and money follow the UI Conventions. | UI Conventions |
| SR-013 | CI/CD approval, rollback and server alerts have no control on this screen. They run in the CI tool and the hosting provider's tools. | FR-06-11, FR-06-12 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-06-01 | The Campaigns tab: list, create, edit and end a campaign, with promotion codes generated on save. |
| FR-06-02 | The Push Notifications tab: compose, choose audience, send now or schedule. |
| FR-06-03 | Shows how many codes were redeemed. The limit itself is enforced at checkout (F-02). |
| FR-06-04 | The Overview tab: visitors by hour and zone, ticket revenue and promotion redemptions for a date range. |
| FR-06-05 | The events collected by the app are the source of the visitor figures. This screen has no control for it. |
| FR-06-09 | The Staff & Roles tab: assign a role to a staff account. |
| FR-06-10 | Every campaign and role change from this screen adds an audit record. |
| FR-01-08 | The role decides which tabs show and which requests are allowed. |
| FR-01-06 | The Log Out button and its confirmation dialog. |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px. The console is designed desktop-first, but every function must work on a phone.
- What collapses or stacks on a narrow screen: tabs scroll sideways; the three cards stack in one column; the chart fills the width; list rows become cards with the label above each value; the campaign form opens as a full-width panel. From 1024px the cards sit in one row, the chart and the two lists sit side by side, and the campaign form opens as a side panel.
- Information never carried by colour alone: the campaign status is written as "Scheduled", "Active" or "Ended" next to an icon. Chart lines differ by pattern and are labelled, and the "View as table" link gives the same numbers as text.
- Text contrast and minimum size: body text contrast at least 4.5:1 and at least 16px. Every button, tab and row action is at least 24 by 24 CSS pixels.
- Tabs can be used with the keyboard (arrow keys move between tabs). Form errors are announced and focus moves to the first error. Toasts are announced to screen readers.