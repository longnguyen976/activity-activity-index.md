# Screen Spec: S-F05-01 Social Feed

| Field | Value |
|---|---|
| Screen ID | `S-F05-01` |
| Screen name | Social Feed |
| Actor | Visitor / GenZ Customer (secondary actor: Staff & Admin, who moderate posts on S-F06-01) |
| Priority | Should (FR-05-02 moderation is Must) |
| Feature | `F-05 Partner Platform & Event Management` |
| Activity diagrams | `AD-UC-11` |
| Mockup image | `img/S-F05-01.png` |
| Status | Draft |

Document Owner: Trần Hương Ly | Last Updated: 2026-11-07

## 1. Purpose

**Shown when:** The visitor opens the Social Gallery tab to browse community photos, tap Post to share a photo, or report a post that breaks the rules.

**The user leaves this screen when:** The visitor taps back to the Main Menu or switches to another tab (for example Events, S-F05-03).

## 2. Mockup

![S-F05-01](img/S-F05-01.png)

Layout overview:

- **Top bar:** Back button | title "Social Gallery" | profile avatar.
- **Main area:** Photo grid of published posts, newest first. Each tile shows the photo, author name, location tag, date and a short caption. The next page loads when the visitor reaches the end of the grid.
- **Bottom right:** Floating primary button "Post a Photo".
- **Overlays:** M-F05-01 Create Post Sheet and M-F05-02 Report Post Dialog.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Page Title | Header | "Social Gallery" (static) | Yes | None |
| 2 | Profile Avatar | Image / Button | Current user avatar, `user.avatar_url` | No | Opens profile S-F01-01 |
| 3 | Post Grid | List | Published posts, newest first (FR-05-03) | Yes | Only posts with status Published are shown |
| 4 | Post Tile | Image | `post.photo_url` (thumbnail from CDN) | Yes | Alt text = caption, or "Photo by [author]" when caption is empty |
| 5 | Author Name | Text | `post.author_name` | Yes | None |
| 6 | Location Tag | Text | `post.location_name` | No | Hidden when empty |
| 7 | Post Date | Text | `post.created_at`, format DD/MM/YYYY HH:mm | Yes | None |
| 8 | Caption | Text | `post.caption` | No | Shows 2 lines, then "…" |
| 9 | Report Button | Secondary button (icon + text "Report") | Static | No | Hidden on the visitor's own posts |
| 10 | Load More Indicator | Loading indicator | Static | No | Shown while the next page loads |
| 11 | Post a Photo Button | Primary button | "Post a Photo" | Yes | Opens M-F05-01 |
| 12 | Empty Message | Banner | "The gallery is empty. Be the first to share a photo!" | No | Shown only when no published posts exist |
| 13 | Photo Picker (in M-F05-01) | Image input | Device gallery or camera | Yes | JPG or PNG; maximum size to be confirmed (see Open Questions) |
| 14 | Caption Input (in M-F05-01) | Text input | Label "Caption" | No | Maximum 300 characters; counter shown below |
| 15 | Location Tag Input (in M-F05-01) | Dropdown | List of park zones | No | Must be one of the listed zones |
| 16 | Submit Post Button (in M-F05-01) | Primary button | "Post" | Yes | Disabled until a photo is selected |
| 17 | Cancel Button (in M-F05-01) | Secondary button | "Cancel" | Yes | Closes the sheet and discards the draft |
| 18 | Report Reason (in M-F05-02) | Radio group | "Inappropriate content", "Spam", "Other" | Yes | One option must be selected |
| 19 | Report Details (in M-F05-02) | Text input | Label "Details (optional)" | No | Maximum 200 characters |
| 20 | Submit Report Button (in M-F05-02) | Primary button | "Submit Report" | Yes | Disabled until a reason is selected |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Photo grid with the first page of published posts, newest first, and the "Post a Photo" button | Visitor opens the tab and posts are found |
| Empty (no data) | Banner "The gallery is empty. Be the first to share a photo!" with the "Post a Photo" button | No published posts exist (FR-05-03) |
| Loading | Skeleton tiles on first load; small spinner below the grid when loading the next page; "Post" button shows a spinner while the photo uploads | Opening the tab, reaching the end of the grid, or submitting a post |
| Error | Banner "We couldn't load the gallery. Check your connection and try again." with a "Retry" button. After a failed upload, a toast "Upload failed. Your photo was not posted. Please try again." and the sheet stays open with the selected photo kept | Feed request fails, or Cloud Storage / CDN cannot be reached |
| Success / confirmation | Toast "Your photo is now live." for a clean post; toast "Your photo is being reviewed before it appears." for a flagged post; toast "Report sent. The post is now hidden while we review it." after a report | Post published, post flagged by the automated filter, or report submitted |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Open the tab | Queries the first page of published posts, newest first (FR-05-03); photos load from the CDN | stays |
| 2 | Post Grid | Scroll to the end of the page | Queries the next page; shows the load indicator | stays |
| 3 | Post a Photo Button | Tap | Opens M-F05-01 | stays (modal) |
| 4 | Photo Picker | Select a photo, write a caption, tag a location | Shows a preview and enables "Post" | stays (modal) |
| 5 | Submit Post Button | Tap "Post" | Uploads the photo to storage (FR-05-01). If upload fails: error toast, no post is created, the visitor can retry. If stored: saves the post with user ID and timestamp, then runs the automated filter (FR-05-02). Not flagged: post is published to the feed. Flagged: post is held hidden and added to the moderation queue | stays (modal closes) |
| 6 | Report Button | Tap | Opens M-F05-02 | stays (modal) |
| 7 | Submit Report Button | Tap "Submit Report" | Hides the reported post at once and adds it to the moderation queue (FR-05-02); shows the success toast | stays |
| 8 | Profile Avatar | Tap | Opens the visitor profile | `S-F01-01` |
| 9 | Events tab | Tap | Opens the event calendar | `S-F05-03` |
| 10 | Back button | Tap | Returns to the previous screen | Main Menu |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only signed-in visitors can post or report. A visitor who is not signed in is sent to S-F00-01. | AD-UC-11 precondition |
| SR-002 | The feed shows only Published posts, newest first. Hidden and Removed posts are never shown. | FR-05-03; AD-UC-11 object states |
| SR-003 | A failed upload creates no post and keeps the selected photo so the visitor can retry. | AD-UC-11 (d1) |
| SR-004 | A post flagged by the automated filter is hidden at once and is never shown first. | FR-05-02 |
| SR-005 | A reported post is hidden immediately and joins the moderation queue before any moderator looks at it. | FR-05-02 |
| SR-006 | If Staff confirm a violation, the post is removed and the author is notified. If not, the post is restored to the feed. | AD-UC-11 (d4) |
| SR-007 | A visitor cannot report their own post. | Screen-level decision |
| SR-008 | Dates are shown as DD/MM/YYYY HH:mm; buttons use verb-first labels. | UI Conventions |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-05-01 | Lets the visitor select a photo, write a caption, tag a location and post it |
| FR-05-02 | Lets the visitor report a post; shows the "being reviewed" message when a post is flagged |
| FR-05-03 | Shows published posts as a photo grid, newest first, with paging and an empty message |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px.
- Mobile: 2-column photo grid; Create Post Sheet opens as a full-height bottom sheet; "Post a Photo" button floats at the bottom right.
- Tablet / desktop web: 3 to 4 column grid with content width capped; modals open as centered dialogs.
- Information never carried by colour alone: post status messages use text, not colour only.
- Every photo has alt text; all buttons have text or accessible labels; tap targets at least 24 by 24 CSS pixels (target 48 by 48 dp on mobile); body text contrast at least 4.5:1.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Banned keywords, maximum photo size, caption length and page size are not in the workbook (AD-UC-11 section 6 #7). Values used here are placeholders.] | No | Open |
| 2 | [NEEDS CLARIFICATION: FR-05-01 and FR-05-03 are Should, FR-05-02 is Must. If only Must is built, moderation exists without posting.] | No | Open |
| 3 | [NEEDS CLARIFICATION: Should the post composer become its own screen (S-F05-xx) instead of a modal sheet, since it has several inputs?] | No | Open |
| 4 | [NEEDS CLARIFICATION: Moderation queue screen for Staff is assumed to be part of S-F06-01 (Hiệp). Please confirm.] | No | Open |
| 5 | [NEEDS CLARIFICATION: Does the reporter keep seeing the reported post hidden for themselves only, or is it hidden for everyone?] | No | Open (drawn as hidden for everyone) |

---

## Completion checklist

- [ ] The mockup is a separate cropped image file `img/S-F05-01.png`.
- [x] Every visible element appears in the element inventory (re-check after the mockup is drawn).
- [x] Every input element has a validation rule.
- [x] All five states are filled in.
- [x] Every navigation target exists in the Screen List (S-F01-01, S-F05-03 pending team confirmation).
- [x] Every element that displays data names its field.
- [x] Every linked FR exists in the Function List.
