## Screen Spec: S-F03-01 Interactive 2.5D Map (Owner: Nguyễn Thị Phúc An)

| Field | Value |
|---|---|
| Screen ID | S-F03-01 |
| Screen name | Interactive 2.5D Map |
| Actor | Visitor / GenZ Customer |
| Priority | Must (FR-03-01 to FR-03-06 are all Must) |
| Feature | F-03 Interactive Map & Park Navigation |
| Activity diagrams | AD-UC-03 (map and location, search and route, periodic wait-time refresh) |
| Mockup image | img/S-F03-01.png |
| Status | Draft |

Document Owner: Nguyễn Thị Phúc An | Last Updated: 2026-11-08

### 1. Purpose

**Shown when:** The visitor opens the park map from the main menu, or taps "Open Park Map" on another screen (for example S-F04-03), to see the park, their own position, wait times and walking directions.

**The user leaves this screen when:** The visitor goes back to the previous screen or the Main Menu, or leaves the map (position updates then stop).

**Source trace:** AD-UC-03 nodes s3, r1 (load map), s1, d1, s2, g1, p1, d3 (location), a2, s7, d2, s8-s11 (search), a3, g2, s12 (route), t2, w1, d4, w2, w3 (wait-time refresh). The Staff update flow (st1, s13, s14, FR-03-06) is **not** on this screen: see Linked requirements.

### 2. Mockup

Mockup image file: img/S-F03-01.png

Sample data only. Attraction names, wait times, walking times and distances in the image are illustrative.

Layout overview:

- **Top area:** Back button | search field "Search attractions or places".
- **Map canvas (full screen):** 2.5D/3D park map with POI markers and a "You are here" marker (only when location is allowed and available).
- **Search results list (below the search field, only while searching):** Matching attractions with location, status, wait time and walking time when available.
- **Attraction detail sheet (bottom, M-F03-01):** Name, location, status (Open / Closed), wait time, walking time and the primary button "Get Directions".
- **Route overlay:** Walking route drawn on the map with a summary of walking time and distance.
- **Bottom navigation:** Shared app navigation (route owner to confirm).

### 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | MapTopBar | Header | Back button and search field | Yes | Back returns to the previous screen |
| 2 | SearchField | Text input | Placeholder "Search attractions or places" (FR-03-03) | Yes | Submits on the keyboard search key; empty input is not submitted |
| 3 | MapCanvas | Map (Map SDK) | 2.5D/3D park map with POIs from the map data (FR-03-01) | Yes | Supports pan, zoom and rotate; POI data comes from the server |
| 4 | PoiMarker | Map marker | One per attraction or POI (FR-03-01) | Yes | Tapping opens the attraction detail sheet |
| 5 | CurrentPositionMarker | Map marker | "You are here" at the visitor's GPS or Beacon position (FR-03-02) | No | Shown only when location permission is granted and valid coordinates exist; moves while the map is open |
| 6 | SearchResultList | List | Matching results: name, location, status, wait time, walking time when available (FR-03-03) | No | Shown only after a search with a match |
| 7 | SearchResultItem | List item | Name, status badge, wait time, walking time | No | Walking time is hidden when no current position is available |
| 8 | NotFoundMessage | Banner | "No attractions found for your search." plus similar names, e.g. "Did you mean: Ferris Wheel?" (FR-03-03) | No | Only on a search with no match; tapping a similar name runs that search |
| 9 | SearchErrorBanner | Banner | "Unable to load data. Please try again." (FR-03-03) | No | Only when the data source returns an error |
| 10 | TryAgainButton | Secondary button | "Try Again" | No | Only with SearchErrorBanner; repeats the search |
| 11 | AttractionNameText | Text | Attraction name | Yes | In the detail sheet |
| 12 | AttractionLocationText | Text | Location in the park | Yes | In the detail sheet |
| 13 | StatusBadge | Badge | "Open" or "Closed" (FR-03-05) | Yes | Text plus icon, not colour alone |
| 14 | WaitTimeBadge | Badge | e.g. "25 min" (FR-03-05) | No | Shown only when status is Open; replaced by StatusBadge "Closed" when Closed; shows an unavailable state when no wait time exists |
| 15 | WalkingTimeText | Text | e.g. "6 min walk" (FR-03-03) | No | Separate from the wait time; hidden when no current position |
| 16 | ClosedMessage | Banner | "Currently closed" (FR-03-03) | No | Only when the attraction is Closed; no route is offered |
| 17 | GetDirectionsButton | Primary button | "Get Directions" (FR-03-04) | No | Shown only for an Open attraction; needs a current position (see Open Question 2) |
| 18 | RouteOverlay | Map layer | Walking route from the current position to the destination (FR-03-04) | No | Calculated on the device by the Map SDK |
| 19 | RouteSummary | Text | Walking time and distance, e.g. "6 min · 450 m" (FR-03-04) | No | Shown with RouteOverlay; unit format to confirm |
| 20 | MapLoadingSkeleton | Placeholder | Map placeholder while map data loads | No | Shown on first open |
| 21 | AttractionDetailSheet (M-F03-01) | Bottom sheet | Elements 11 to 17 | No | Opens when a POI marker or a search result is tapped; closes on swipe down |
| 22 | BottomNavigation | Shared app navigation | Shared shell | Yes | Route owner to confirm |

### 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Map with POI markers, search field, "You are here" marker when location is allowed | Visitor opens the screen and map data loads |
| Empty (no data) | NotFoundMessage with similar names instead of results; the map stays visible | Search has no match |
| Loading | MapLoadingSkeleton on first open; small spinner in the result list while a search runs; wait time shows the last value until the refresh returns | Screen opens; visitor submits a search; periodic refresh |
| Error | Search failure: SearchErrorBanner "Unable to load data. Please try again." with TryAgainButton. The map and POIs stay visible | Search data source returns an error |
| Success / confirmation | Search with a match: result with location, status, wait time, walking time. Open attraction: "Get Directions" shows RouteOverlay and RouteSummary. Closed attraction: ClosedMessage and no route | Search matches; visitor requests directions on an Open attraction |

Other situations on this screen: **location permission denied** shows the map and POIs without CurrentPositionMarker and without a prompt (see Open Question 1). Map data failing to load at open is not defined (Open Question 3).

### 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Screen | Open the screen | Loads map data and POI list, renders the 2.5D/3D map (FR-03-01), then requests location permission (FR-03-02) | stays |
| 2 | Location permission prompt (system) | Allow | Reads the position from GPS or Beacon and shows CurrentPositionMarker; updates it while the map is open (FR-03-02) | stays |
| 3 | Location permission prompt (system) | Deny | Keeps the map and POIs, no CurrentPositionMarker, no position updates | stays |
| 4 | SearchField | Submit a search | Queries POI and attraction data (FR-03-03). Match, Open: shows location, status, wait time and walking time. Match, Closed: shows "Currently closed". No match: shows NotFoundMessage with similar names. Data error: shows SearchErrorBanner | stays |
| 5 | NotFoundMessage similar name | Tap a similar name | Runs the search again with that name | stays |
| 6 | TryAgainButton | Tap | Repeats the last search | stays |
| 7 | PoiMarker or SearchResultItem | Tap | Opens the attraction detail sheet (M-F03-01) with the latest wait time and status (FR-03-05) | stays |
| 8 | AttractionDetailSheet | Stay on an attraction | Refreshes wait time and status periodically while the attraction is viewed. Open: shows wait time and status Open. Closed: shows status Closed instead of a wait time (FR-03-05) | stays |
| 9 | GetDirectionsButton | Tap | The Map SDK calculates the walking route on the device; shows RouteOverlay with walking time and distance (FR-03-04) | stays |
| 10 | Back button | Tap | Leaves the map; position updates stop | Previous screen |

### 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | On open, the map shows the 2.5D/3D park map with POIs and attraction information. Wait time is not shown at map load; it appears in search results and the detail sheet. | FR-03-01; AD-UC-03 s3, r1 |
| SR-002 | The current-position marker is shown only when location permission is granted and valid GPS or Beacon coordinates exist. It is updated while the map is open and stops when the visitor leaves the map. | FR-03-02; AD-UC-03 d1, d3 |
| SR-003 | If location permission is denied, the map and POIs are still shown, with no position marker and no prompt to enable location (default applied). | FR-03-02; AD-UC-03 section 6 #1 |
| SR-004 | A search returns matching results with location, status, wait time and walking time when available. No match shows a not-found message with similar names. | FR-03-03; AD-UC-03 d2 |
| SR-005 | A Closed attraction shows "Currently closed" and no route is offered. | FR-03-03; AD-UC-03 s10 |
| SR-006 | Directions are walking routes only, calculated on the device from the current position. Walking time and wait time are separate values. | FR-03-04; AD-UC-03 g2 |
| SR-007 | While an attraction is viewed, wait time and status refresh periodically. A Closed status replaces the wait time. | FR-03-05; AD-UC-03 t2, d4 |
| SR-008 | A Staff update becomes visible to visitors at their next read, not instantly. Staff cannot edit wait time or status on this screen. | FR-03-06; AD-UC-03 s13 |
| SR-009 | Dates use DD/MM/YYYY, time HH:mm; buttons use verb-first labels; one primary button per screen ("Get Directions"). | UI Conventions |

### 7. Linked requirements

| FR ID | Priority | What this screen does for it |
|---|---|---|
| FR-03-01 | Must | Shows the 2.5D/3D park map with POI markers and attraction information |
| FR-03-02 | Must | Shows the "You are here" marker and updates it while the map is open |
| FR-03-03 | Must | Provides the search field and the result list with location, status, wait time and walking time; shows not-found, closed and error messages |
| FR-03-04 | Must | Shows the walking route with walking time and distance |
| FR-03-05 | Must | Shows the latest wait time and status in the detail sheet and refreshes them periodically |
| FR-03-06 | Must | Not on this screen. The Staff update form is on a staff screen (not yet assigned, see Open Question 5). This screen only shows the saved result (SR-008) |

### 8. Responsive and accessibility notes

- Smallest supported width: 360px. The map fills the screen; the detail sheet and search results stack above the bottom navigation.
- Touch targets are at least 48x48dp, including POI markers and the "Get Directions" button.
- POI markers, StatusBadge and WaitTimeBadge have text labels, not colour alone. Screen readers read the attraction name, status and wait time.
- Walking route and "You are here" marker have text alternatives (RouteSummary, "You are here" label).
- Text supports resizing without hiding the primary button.

### 9. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | FR-03-02 says only "WHILE location permission is available". What happens when permission is denied, and does the app prompt the visitor to enable location? (AD-UC-03 #1) | No | Map and POIs shown without a position marker; no prompt |
| 2 | FR-03-04 needs a current location. What does the visitor get when location is unavailable (permission denied or no GPS fix)? (AD-UC-03 #4) | Yes | No route is offered; "Get Directions" is hidden or disabled. Behaviour not defined by any FR |
| 3 | What happens if map data cannot be loaded at open, or GPS or Beacon returns no position? FR-03-03 covers errors only for search. (AD-UC-03 #5) | No | Not designed |
| 4 | What does Staff see when an update is rejected (FR-03-06 enforces staff authorisation)? (AD-UC-03 #8) | No | Not on this screen |
| 5 | Which screen holds the Staff form for FR-03-06 (wait time and status)? S-F06-01 Admin Dashboard (owner Lê Thành Hiệp), or a new screen? | Yes | None; FR-03-06 is not covered by any screen yet |
| 6 | Which screens open S-F03-01 besides S-F04-03 (Main Menu, Home)? Is sign-in required to view the map? | No | Entry from the Main Menu and S-F04-03 "Open Park Map" |
| 7 | What are the refresh intervals for position and wait time, and the display units for walking time and distance? (AD-UC-03 #3) | No | "While map open" and "periodic"; examples only |
