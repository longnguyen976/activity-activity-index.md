# Screen Spec: S-F00-01 Sign In

| Field | Value |
|---|---|
| Screen ID | `S-F00-01` |
| Screen name | Sign In |
| Actor | Visitor / GenZ Customer; Marketing & Sales Dept; IT & Technical Dept (every signed-out user) |
| Priority | Must (Continue with Google is Should) |
| Feature | `F-00 Shared screens` (serves `F-01 Identity & User Management`) |
| Activity diagrams | `AD-UC-01` |
| Mockup image | `img/S-F00-01.png` |
| Status | Draft |

## 1. Purpose

**Shown when:** a user without a valid session opens the app, has just logged out, or has just reset a password (the user must sign in again).

**The user leaves this screen when:** the user signs in or creates an account (goes to Home, or to S-F06-01 for staff), or closes the app. Moving between the panels below does not leave the screen.

## 2. Mockup

![S-F00-01](img/S-F00-01.png)

*Image placeholder: export the design frame to `img/S-F00-01.png` (panel A, the default). Panels B to E are saved as `img/S-F00-01-B.png` to `img/S-F00-01-E.png`.*

The screen has five panels that replace each other inside one card:

- **A. Sign In** (default): identifier, password, Sign In, Continue with Google, links to Forgot Password and Create Account.
- **B. Create Account:** identifier, password, terms consent, Create Account.
- **C. Enter Code:** 6-digit code, resend countdown, Verify Code. Used by B and D.
- **D. Forgot Password:** registered identifier, Send Code.
- **E. Set New Password:** new password, Reset Password, then a confirmation.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | App Header | Header | Static: MySen logo and the title of the current panel ("Sign In", "Create Account", "Enter Code", "Forgot Password", "Set New Password") | Yes | None |
| 2 | Sign-in Identifier | Text input | Label "Phone number or email". Matched against User account & profile (FR-01-03) | Yes | Phone `0xxx xxx xxx` (10 digits starting with 0, spaces optional) or a valid email. Error: "Enter a valid phone number or email address." |
| 3 | Sign-in Password | Text input (masked) | Label "Password". Never written to logs or shown after submit (FR-01-09) | Yes | Not empty. Error: "Enter your password." |
| 4 | Show/Hide Password | Toggle | Eye icon, accessible label "Show password" / "Hide password". Used on every password field | No | Default hidden |
| 5 | Sign In Button | Primary button | "Sign In" (panel A) | n/a | Enabled when #2 and #3 are filled. Disabled while loading or while the account is locked |
| 6 | Lock-out Banner | Banner | "Too many failed attempts. Try again in {mm:ss}." with a lock icon. Shown only while the account is locked (FR-01-11) | No | Countdown starts from the remaining time returned by the server; at most 15 minutes |
| 7 | Credentials Error | Text | "Incorrect phone number, email or password. Check your details and try again." Shown under the form after a failed sign-in | No | Same message for a wrong identifier and a wrong password |
| 8 | Google Sign-In Button | Secondary button | "Continue with Google" (FR-01-04, Should) | n/a | None |
| 9 | Forgot Password Link | Button (text link) | "Forgot Password?" → panel D | n/a | None |
| 10 | Create Account Link | Button (text link) | "Create Account" → panel B | n/a | None |
| 11 | Register Identifier | Text input | Label "Phone number or email" (panel B). Checked against User account & profile (FR-01-01) | Yes | Same format rule as #2. If already registered: "This phone number or email is already registered. Sign in instead." No code is sent |
| 12 | Register Password | Text input (masked) | Label "Password" (panel B) | Yes | Meets the password policy [NEEDS CLARIFICATION: Q-01]. Error: "Your password doesn't meet the requirements. Check the rules shown below the field." |
| 13 | Terms Consent | Checkbox | "I agree to the Terms of Use" with a link. Stores a consent record when the account is created (FR-01-01) | Yes | Must be ticked. Error: "Accept the Terms of Use to continue." |
| 14 | Create Account Button | Primary button | "Create Account" (panel B) | n/a | Enabled when #11 to #13 are filled. On success the code panel opens |
| 15 | Back to Sign In Link | Button (text link) | "Back to Sign In" on panels B, C and D → panel A | n/a | None |
| 16 | Code Destination Hint | Text | "We sent a 6-digit code to {masked phone or email}." | Yes | None |
| 17 | Code Input | Text input | Label "6-digit code" (panel C). Numeric keypad | Yes | Exactly 6 digits, valid for 5 minutes. Error: "That code is incorrect or has expired. Check the code or request a new one." |
| 18 | Resend Code | Button (text link) | "Resend code in {ss}s", then "Resend Code" when the countdown ends (FR-01-01). A new code makes the old one invalid | No | Disabled while the countdown runs [NEEDS CLARIFICATION: Q-02] |
| 19 | Verify Code Button | Primary button | "Verify Code" (panel C) | n/a | Enabled when 6 digits are entered |
| 20 | Code Delivery Error | Banner | "We couldn't send the code. Try again." with a "Resend Code" action (FR-01-02) | No | Shown when the SMS or email provider fails. Does not count as a failed attempt |
| 21 | Reset Identifier | Text input | Label "Registered phone number or email" (panel D) | Yes | Same format rule as #2 |
| 22 | Send Code Button | Primary button | "Send Code" (panel D) | n/a | Enabled when #21 is valid. The next panel looks the same whether or not the account exists |
| 23 | New Password | Text input (masked) | Label "New password" (panel E) | Yes | Meets the password policy [NEEDS CLARIFICATION: Q-01]. Error as in #12 |
| 24 | Reset Password Button | Primary button | "Reset Password" (panel E) | n/a | Enabled when #23 is filled |
| 25 | Password Changed Message | Banner | "Your password was changed. Sign in with your new password." with a "Sign In" button → panel A (FR-01-05) | No | Shown only after a successful reset |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Panel A with empty fields. Sign In is disabled until both fields are filled. No banner or error text | The screen opens |
| Empty (no data) | Not applicable: this is a form screen and shows no list or saved data | n/a |
| Loading | The tapped primary button shows a spinner and is disabled. Fields become read-only. After "Continue with Google" the screen shows "Opening Google..." | The user taps Sign In, Create Account, Send Code, Verify Code, Reset Password or Continue with Google |
| Error | Inline error under the field for format problems. Credentials Error (#7) after a failed sign-in. Lock-out Banner (#6) when locked. Code Delivery Error (#20) when the code could not be sent. Code error under #17. "Google sign-in didn't work. Try again or sign in with your password." after a cancelled or invalid Google sign-in. "We can't connect right now. Check your connection and try again." when offline | Validation fails, the server rejects the request, the provider fails, or the network is down |
| Success / confirmation | Sign-in or registration success: the screen closes and the user lands on the next screen. Password reset success: Password Changed Message (#25) | The server accepts the sign-in, the new account, or the new password |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Opens the app without a valid session | Shows panel A. A silent token refresh has already failed or no token exists (FR-01-10) | stays |
| 2 | Sign In Button (#5) | Taps with a valid identifier and the correct password | Checks the lock status, verifies the password, issues the access and refresh tokens, opens the session (FR-01-03, FR-01-10, FR-01-11) | Home (visitor, partner) or `S-F06-01` (staff) [NEEDS CLARIFICATION: Q-03] |
| 3 | Sign In Button (#5) | Taps with a wrong identifier or password | Shows #7. Adds 1 to the failed-attempt counter. The 5th consecutive failure blocks the account for 15 minutes and shows #6 (FR-01-11) | stays |
| 4 | Sign In Button (#5) | Taps while the account is locked | Rejects at once without checking the password. #6 shows the remaining time (FR-01-11) | stays |
| 5 | Google Sign-In Button (#8) | Taps | Redirects to Google. After Google confirms, the server verifies the ID token and signs in the linked account, or creates one (FR-01-04) | Home or `S-F06-01`; on cancel or invalid token: shows the Google error and stays |
| 6 | Create Account Link (#10) | Taps | Shows panel B | stays |
| 7 | Create Account Button (#14) | Taps with valid data | Validates, holds the pending registration, generates a 6-digit code valid for 5 minutes and sends it by SMS or email (FR-01-01, FR-01-02) | stays (panel C) |
| 8 | Create Account Button (#14) | Taps with invalid data or an identifier already registered | Shows the error, sends no code, keeps what was typed (FR-01-01) | stays |
| 9 | Verify Code Button (#19) | Enters the correct code in time (registration) | Marks the code used, creates the active account with the consent record, opens a session (FR-01-01, FR-01-02, FR-01-10) | Home |
| 10 | Verify Code Button (#19) | Enters the correct code in time (password reset) | Marks the code used and opens the new-password step | stays (panel E) |
| 11 | Verify Code Button (#19) | Enters a wrong or expired code | Rejects it and adds 1 to the failed-attempt counter. At 5 consecutive failures the account is blocked for 15 minutes. Otherwise a new code can be requested (FR-01-02, FR-01-11) | stays |
| 12 | Resend Code (#18) | Taps after the countdown | Sends a new code and invalidates the old one | stays |
| 13 | Forgot Password Link (#9) | Taps | Shows panel D | stays |
| 14 | Send Code Button (#22) | Taps | Holds the reset request and sends a code (FR-01-05, FR-01-02) | stays (panel C) |
| 15 | Reset Password Button (#24) | Submits a valid new password | Replaces the password with a new salted hash, signs out all other sessions, shows #25 (FR-01-05, FR-01-09) | stays (the user taps Sign In to return to panel A) |
| 16 | Reset Password Button (#24) | Submits a password that fails the policy | Shows the policy error and keeps the old password | stays |
| 17 | Back to Sign In Link (#15) | Taps | Returns to panel A and discards what was typed in B, C or D | stays |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | A sign-in succeeds only for an active account with the correct password. | FR-01-03 |
| SR-002 | The sign-in error is the same for a wrong identifier and a wrong password, so it does not reveal which accounts exist. | FR-01-03, AD-UC-01 BT-01-22 |
| SR-003 | After 5 consecutive failed sign-in or code attempts on the same account, further attempts are blocked for 15 minutes. While locked, even the correct password is rejected and the remaining time is shown. | FR-01-11, AD-UC-01 BT-01-17 |
| SR-004 | A code is 6 digits and valid for 5 minutes. A new code makes the previous one invalid. | FR-01-02, AD-UC-01 A-02 |
| SR-005 | Registration needs a valid identifier that is not yet registered, a password that meets the policy, and ticked consent. If any check fails, no code is sent. | FR-01-01, AD-UC-01 BT-01-05 |
| SR-006 | A password is never shown unless the user turns on Show Password, and is never written to logs or returned in any response. | FR-01-09 |
| SR-007 | A cancelled or invalid Google sign-in creates no session and no account. | FR-01-04, AD-UC-01 BT-01-24 |
| SR-008 | After a password reset, all other sessions of the account are signed out and the user must sign in again. | FR-01-05, AD-UC-01 A-06 |
| SR-009 | Tokens are kept in secure storage and refreshed silently. If the refresh fails, the user returns to this screen. | FR-01-10 |
| SR-010 | After sign-in the destination depends on the role of the account. | FR-01-08 |
| SR-011 | If the SMS or email provider fails, the user can ask for a new code. The failure does not count toward the lock-out. | FR-01-02, AD-UC-01 A-05 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-01 | Panel B collects identifier, password and consent. Panel C verifies the code. |
| FR-01-02 | Panel C shows the code input, the resend countdown and the delivery error. |
| FR-01-03 | Panel A is the sign-in form with show/hide password, the error and the lock-out message. |
| FR-01-04 | The Continue with Google button and the Google error message. |
| FR-01-05 | Panels D and E: forgot password, new password and the confirmation. |
| FR-01-06 | The screen the user returns to after Log Out. |
| FR-01-08 | The role of the account decides where the user lands after sign-in. |
| FR-01-09 | Password fields are masked and never logged or echoed. |
| FR-01-10 | The app keeps tokens in secure storage and returns to this screen when the refresh fails. |
| FR-01-11 | The lock-out banner and the 5-failure rule. |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px.
- What collapses or stacks on a narrow screen: the card fills the width with 16px margins and all fields, buttons and links stack in one column. From 600px the card is centred and at most 420px wide.
- Information never carried by colour alone: errors and the lock-out banner always have an icon and text. A field in error also gets an error message, not only a red border.
- Text contrast and minimum size: body text contrast at least 4.5:1 and at least 16px. Every button, link and the Show/Hide toggle are at least 24 by 24 CSS pixels.
- The code field uses a numeric keypad and the one-time-code autofill hint. Focus moves to the first error after a failed submit. The lock-out countdown is announced to screen readers when it starts, not every second.