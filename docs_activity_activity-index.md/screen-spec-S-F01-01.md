# Screen Spec: S-F01-01 Profile

| Field | Value |
|---|---|
| Screen ID | `S-F01-01` |
| Screen name | Profile |
| Actor | Visitor / GenZ Customer |
| Priority | Must |
| Feature | `F-01 Identity & User Management` |
| Activity diagrams | `AD-UC-01` (sign-out tail: Select Log out, Return to the sign-in screen). FR-01-07 is not drawn in AD-UC-01 (see section 4.3 of that file). |
| Mockup image | `img/S-F01-01.png` |
| Status | Draft |

## 1. Purpose

**Shown when:** a signed-in visitor opens Profile from the main navigation to see their account details, edit their name or date of birth, or log out.

**The user leaves this screen when:** the user taps Back (returns to Home), confirms Log Out (goes to S-F00-01), or the session expires (goes to S-F00-01).

## 2. Mockup

![S-F01-01](img/S-F01-01.png)

*Image placeholder: export the design frame to `img/S-F01-01.png` (view mode). The edit mode is saved as `img/S-F01-01-edit.png`.*

The screen is one column. From top to bottom: header with Back, the account details (full name, date of birth, phone, email), the Edit Profile button, and the Log Out button at the bottom. In edit mode the Edit Profile button is replaced by Save Changes and Cancel.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Screen Header | Header | Title "Profile" | Yes | None |
| 2 | Back Button | Button | Icon with accessible label "Back" | Yes | Returns to the previous screen (Home) |
| 3 | Full Name | Text input (read-only in view mode) | Label "Full name". Field `name` of User account & profile (FR-01-07) | Yes, when saving | Not blank, 1 to 100 characters, letters (Vietnamese diacritics allowed), spaces, hyphens and apostrophes [NEEDS CLARIFICATION: Q-01]. Error: "Enter your full name." |
| 4 | Date of Birth | Text input with date picker (read-only in view mode) | Label "Date of birth", format `DD/MM/YYYY`. Field `date_of_birth` of User account & profile (FR-01-07) | No | A real date, not in the future, not more than 120 years ago [NEEDS CLARIFICATION: Q-01]. Error: "Enter a valid date of birth in DD/MM/YYYY format." |
| 5 | Phone Number | Text (read-only) | Label "Phone number", format `0xxx xxx xxx`, or "Not added". Field `phone` of User account & profile | No | None. Changing the phone is out of scope for release 1 |
| 6 | Email | Text (read-only) | Label "Email", or "Not added". Field `email` of User account & profile | No | None. Changing the email is out of scope for release 1 |
| 7 | Edit Profile Button | Primary button | "Edit Profile". Visible in view mode only | n/a | None |
| 8 | Save Changes Button | Primary button | "Save Changes". Visible in edit mode only | n/a | Enabled only when at least one value changed and all fields are valid |
| 9 | Cancel Button | Secondary button | "Cancel". Visible in edit mode only | n/a | Discards the unsaved changes and returns to view mode |
| 10 | Log Out Button | Secondary button | "Log Out" (FR-01-06) | n/a | Opens M-F01-01 |
| 11 | Profile Load Error | Banner | "We couldn't load your profile. Check your connection and try again." with a "Try Again" button | No | Shown only when loading fails |
| 12 | Saved Confirmation | Toast | "Profile saved." | No | Disappears by itself after 3 seconds |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | View mode: name, date of birth, phone and email as saved. Phone and email are read-only. Edit Profile and Log Out are visible | The profile loaded successfully |
| Empty (no data) | Name and date of birth show the placeholders "Add your full name" and "Add your date of birth", with the line "Complete your profile." Phone or email shows "Not added" if missing | A new account has no name or date of birth yet (registration collects neither) |
| Loading | Grey placeholder bars for the four details. In edit mode, Save Changes shows a spinner and is disabled | The screen opens, or the user taps Save Changes |
| Error | Profile Load Error (#11) with Try Again. After a failed save: "We couldn't save your changes. Your changes are still here. Try again." Inline field errors as in the element table. If the session expired, the user goes to S-F00-01 with the message "Your session has ended. Sign in again." | The profile call fails, the save fails, a field is invalid, or the session is no longer valid |
| Success / confirmation | The screen returns to view mode with the updated values and shows the toast "Profile saved." | The server stores the changes |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Opens Profile | Reads the profile of the signed-in user (own data only) and shows it (FR-01-07) | stays |
| 2 | Edit Profile Button (#7) | Taps | Makes Full Name and Date of Birth editable. Phone and email stay read-only | stays |
| 3 | Full Name / Date of Birth (#3, #4) | Types, then leaves the field | Checks the value and shows or clears the inline error | stays |
| 4 | Save Changes Button (#8) | Taps with valid values | Validates again and stores the changes. Returns to view mode and shows the updated profile and #12 (FR-01-07) | stays |
| 5 | Save Changes Button (#8) | Taps and the save fails | Keeps the typed values and shows the save error | stays |
| 6 | Cancel Button (#9) | Taps | Discards the typed values and returns to view mode | stays |
| 7 | Log Out Button (#10) | Taps | Opens M-F01-01 | stays |
| 8 | M-F01-01 | Confirms "Log Out" | Ends the session on this device and revokes the current refresh token. Other devices stay signed in (FR-01-06) | `S-F00-01` |
| 9 | M-F01-01 | Taps "Cancel" | Closes the dialog | stays |
| 10 | Back Button (#2) | Taps | Returns to the previous screen. In edit mode with unsaved changes the changes are discarded | Home (ID to be confirmed, Q-02) |
| 11 | Screen | The access token expires and the silent refresh fails | Ends the session and shows the sign-in screen (FR-01-10) | `S-F00-01` |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only the name and the date of birth can be edited. Phone and email are shown read-only. | FR-01-07, Scope Table 3 |
| SR-002 | A save stores the changes and then shows the updated profile. | FR-01-07 |
| SR-003 | A user can read and save only their own profile. | FR-01-07, FR-01-08 |
| SR-004 | Log Out always asks for confirmation. It ends the session on this device only and returns to the sign-in screen. | FR-01-06 |
| SR-005 | The screen never shows or changes the password. Changing a password is out of scope for release 1 (the forgot-password flow covers lost access). | Scope Table 3, FR-01-09 |
| SR-006 | Dates use `DD/MM/YYYY` and phone numbers use `0xxx xxx xxx`. | UI Conventions |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-07 | The profile view and edit form: name and date of birth editable, phone and email read-only. |
| FR-01-06 | The Log Out button with its confirmation dialog. |
| FR-01-08 | The user can reach and change only their own profile; other requests are refused with an authorisation error. |
| FR-01-10 | The screen depends on a valid session. When the refresh fails the user returns to sign in. |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px.
- What collapses or stacks on a narrow screen: one column with 16px margins; each label sits above its value. From 600px the content is centred in a column at most 560px wide. Save Changes and Cancel sit side by side from 600px and stack below that.
- Information never carried by colour alone: field errors have an icon and a message. Read-only fields are marked with the label "(read-only)" for screen readers and a lock icon, not only a grey tint.
- Text contrast and minimum size: body text contrast at least 4.5:1 and at least 16px. All buttons and the Back icon are at least 24 by 24 CSS pixels.
- The date field accepts typing in `DD/MM/YYYY` as well as the date picker. The toast is announced to screen readers.