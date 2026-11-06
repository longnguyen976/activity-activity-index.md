# Screen List: <Product name>

<!--
DBIZ2 Lesson 8 template. File: docs/external-design/screen-list.md
One row per screen. A screen is a place the user navigates to (a page or route). Loading, empty,
error and success are STATES of a screen and are written in its Screen Spec, not here. A dialog
that only confirms or cancels an action (for example "Delete this post?") is a MODAL of the screen
that opens it, not a separate screen.
-->

Screen ID pattern: `S-<feature>-<nn>`, for example `S-F01-02` is the second screen of feature F-01.
Screens shared by every feature (sign in, settings) use `S-F00-<nn>`.

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F00-01 | | | F-00 | | | | Must | `screens/screen-spec-S-F00-01.md` |
| S-F01-01 | | | F-01 | | | | Must | `screens/screen-spec-S-F01-01.md` |

## Modals and overlays

<!-- List them here so nobody adds them to the table above by mistake. -->

| Modal | Opened from screen | Purpose |
|---|---|---|
| | | |

## Checks before you save

- [ ] Every Screen ID is unique. No ID is used for two different screens.
- [ ] No row is a state (error, empty, loading, success) or a confirmation modal.
- [ ] Every Must use case has at least one screen, or a note saying it has no user interface
      (for example a time-triggered activity).
- [ ] Every actor action in the activity diagrams maps to a screen in this list.
- [ ] Every screen serves at least one FR. A screen that serves none is out of scope or a missing FR.
