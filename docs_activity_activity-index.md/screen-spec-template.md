# Screen Spec: <Screen ID> <Screen name>

<!--
DBIZ2 Lesson 8 template. One file per screen: docs/external-design/screens/screen-spec-<Screen ID>.md
The same template is used in DBIZ3 (Session 4), so what you write now is what the coding agent
reads next semester. The mockup image stays an image; everything around it is text.
-->

| Field | Value |
|---|---|
| Screen ID | `S-<Fxx>-<nn>` from docs/external-design/screen-list.md |
| Screen name | |
| Actor | |
| Priority | Must / Should / Could (from the Screen List) |
| Feature | `<F-ID> <feature name>` |
| Activity diagrams | `AD-<UC-ID>` where this screen appears |
| Mockup image | `img/<Screen ID>.png` |
| Status | Draft / Reviewed |

## 1. Purpose

**Shown when:** <the trigger, in one sentence>

**The user leaves this screen when:** <the exit condition>

## 2. Mockup

![<Screen ID>](img/<Screen ID>.png)

<!-- The image is the visual contract: spacing, grouping, hierarchy. The tables below are the
     behavioural contract. Crop one screen per file. Do not paste a sheet of twelve screens. -->

## 3. Element inventory

<!-- Walk the mockup top to bottom, left to right. Every element a user can see or touch gets a
     row. Give each one a stable name; the coding agent will use it as the component name.
     Data source: a data group from W0 Table 4 (for example "Bookings"), with the FR. -->

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | | Header / Text / Input / Button / List / Map / Image / Toggle | static text, or data field name | Yes / No | |

## 4. States

<!-- The mockup usually shows only the happy state. The other four are where products break. -->

| State | What the user sees | Trigger |
|---|---|---|
| Default | | |
| Empty (no data) | | |
| Loading | | |
| Error | | |
| Success / confirmation | | |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | | tap / type / scan | | `<Screen ID>` or "stays" |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | | FR / BR / state |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-xx-xx | |

## 8. Responsive and accessibility notes

- Smallest supported width: <e.g. 360px>
- What collapses or stacks on a narrow screen:
- Information never carried by colour alone:
- Text contrast and minimum size:

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: ...] | | Open |

---

## Completion checklist

- [ ] The mockup is a separate cropped image file, named with the Screen ID.
- [ ] Every visible element in the mockup appears in the element inventory.
- [ ] Every input element has a validation rule or an explicit "none".
- [ ] All five states are filled in, or marked "not applicable" with a reason.
- [ ] Every navigation target is a Screen ID that exists in the Screen List.
- [ ] Every element that displays data names the field it displays.
- [ ] Every linked FR exists in the Function List, and every action of this screen in an activity
      diagram is covered by an interaction row.

---

Template source: DBIZ3 Session 4 Screen Spec template (VJCBI College - FTU, 2026), adopted
unchanged in structure for DBIZ2 Lesson 8, with Feature and Activity diagram fields added.
