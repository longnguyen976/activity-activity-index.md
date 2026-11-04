# Activity Diagram: AD-UC-03 Interactive Map & Navigation

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-03 |
| Use case | UC-03 Interactive Map & Navigation |
| Feature | F-03 Interactive Map & Park Navigation |
| Primary actor | Visitor / GenZ Customer (secondary actor: Staff & Admin, who updates wait time and status) |
| Lanes | Visitor / GenZ Customer · Staff & Admin · MySen App Platform · Map SDK & GPS Service |
| Inputs (L3-L5) | Context Diagram v<n> · Use Case Diagram v<n> · Sequence: `S-03` (F-03 sequence diagram, 41 steps) · W1 rows FR-03-01 to FR-03-06 |
| Trigger | The visitor opens the interactive map (main flow). Staff submits a wait-time or status update (Staff flow). |
| Preconditions | The MySen app is installed and opened. Map, POI and attraction data exist in the database. |
| Success end state | The visitor sees the map with POIs, wait times and (if allowed) their current position; or sees the route, walking time and distance to an open attraction. Staff sees the update confirmed and the new wait time and status are stored for visitors. |
| Failure end state(s) | Location permission denied: map is shown without the current-position marker. Search data unavailable: the visitor sees an error message. Attraction closed: the visitor sees "Currently closed". |
| FRs covered | `FR-03-01`, `FR-03-02`, `FR-03-03`, `FR-03-04`, `FR-03-05`, `FR-03-06` |
| Business rules applied | none found in the workbook \[NEEDS CLARIFICATION: see section 6 \#2\] |
| Version / status | v0.1 · Draft |
| Author | Nguyễn Thị Phúc An (owner of F-03) |

## 1\. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`), Pattern A (`swimlane-beta`, needs Mermaid 11.16 or later).&nbsp;

```mermaid
swimlane-beta TB
  subgraph VIS["Visitor / GenZ Customer"]
    start@{ shape: sm-circ }
    a1("Open interactive map")
    a2("Search attraction or POI")
    a3("Select destination and ask for directions")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
    stop3@{ shape: fr-circ }
  end
  subgraph STF["Staff & Admin"]
    start2@{ shape: sm-circ }
    st1("Update wait time and open or closed status")
    st2("See update confirmed")
    stop6@{ shape: fr-circ }
  end
  subgraph SYS["MySen App Platform"]
    s3("Load map data and POI list (FR-03-01)")
    s1("Request location permission (FR-03-02)")
    d1{"Location permission<br>granted?"}
    s2("Keep map without current position (FR-03-02)")
    f1@{ shape: fork }
    m1{" "}
    s7("Query POI and attraction data (FR-03-03)")
    d2{"Search result?"}
    s8("Show error message: unable to load data, try again (FR-03-03)")
    s9("Show not-found message and similar names (FR-03-03)")
    s10("Show message: currently closed (FR-03-03)")
    s11("Show location, status, wait time and walking time (FR-03-03)")
    s12("Show route, walking time and distance (FR-03-04)")
    s13("Save wait time and status (FR-03-06)")
    s14("Confirm update to Staff (FR-03-06)")
    t2@{ shape: hourglass }
    w1("Get latest wait time and status (FR-03-05)")
    d4{"Attraction<br>status?"}
    w2("Show wait time and status Open (FR-03-05)")
    w3("Show status Closed (FR-03-05)")
    stop5@{ shape: fr-circ }
  end
  subgraph SDK["Map SDK & GPS Service"]
    r1("Render 2.5D/3D map with POIs (FR-03-01)")
    mloc{" "}
    g1("Get current position from GPS or Beacon (FR-03-02)")
    p1("Update current position on map (FR-03-02)")
    d3{"Map still<br>open?"}
    x1@{ shape: cross-circ }
    g2("Calculate walking route, time and distance on device (FR-03-04)")
  end
  start --> a1 --> s3 --> r1 --> s1 --> d1
  d1 -->|"[denied]"| s2 --> m1
  d1 -->|"[granted]"| f1
  f1 --> mloc --> g1 --> p1 --> d3
  f1 --> m1
  m1 --> a2 --> s7 --> d2
  d2 -->|"[data unavailable]"| s8 --> stop2
  d2 -->|"[no match]"| s9 --> m1
  d2 -->|"[match, attraction Closed]"| s10 --> stop3
  d2 -->|"[match, attraction Open]"| s11 --> a3 --> g2 --> s12 --> stop1
  t2 -->|"periodic refresh while an attraction is viewed"| w1 --> d4
  d4 -->|"[Open]"| w2 --> stop5
  d4 -->|"[Closed]"| w3 --> stop5
  start2 --> st1 --> s13 --> s14 --> st2 --> stop6
  d3 -->|"[map closed]"| x1
  d3 -->|"[map open]"| mloc
```

&nbsp;

Reading notes:

- **Three flows, three initial nodes.** (1) Main flow from `start` (map, location, search, route). (2) Periodic wait-time refresh from the time event `t2`. (3) Staff update from `start2`. The Staff flow and the visitor flows are **not connected by a control flow**. They depend on each other only through stored attraction data: `s13` writes wait time and status; `s7` and `w1` read them. A visitor therefore sees a Staff update at the next read, not instantly.  
- **Fork `f1` without a join.** After permission is granted, the position-update loop (`mloc` → `d3`) runs in parallel with the search flow. The loop branch ends in the flow final `x1` when the map is closed. The other branch ends in `stop1`, `stop2` or `stop3`, which end the whole activity.  
- **Merge `m1`** receives three flows: permission denied, the parallel branch from the fork, and the loop back from "not found". `mloc` is the merge of the position loop. Wait time appears only in `s11` and `w2` (FR-03-03, FR-03-05), not at map load, because FR-03-01 covers map and POIs only.  
- **Routing is on the device.** `g2` is in the Map SDK lane because the sequence diagram notes that the Map SDK handles routing client-side. Walking time and wait time are separate values, as in the sequence diagram note.  
- **Lane mapping.** Database and Backend of the sequence diagram are inside "MySen App Platform" (black-box view). Location Service and Map SDK are merged into "Map SDK & GPS Service". "Operation / Ride Staff" of the sequence diagram is "Staff & Admin" here.

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| s3 | Action | Load map data and POI list | FR-03-01 |
| r1 | Action (SDK lane) | Render 2.5D/3D map with POIs | FR-03-01 |
| s1 | Action | Request location permission | FR-03-02 (FE row: request permission/state) |
| d1 | Decision | Location permission granted? | FR-03-02 (condition "permission is available") |
| s2 | Action | Keep map without current position | FR-03-02 (see section 6 \#1) |
| g1 | Action (SDK lane) | Get current position from GPS or Beacon | FR-03-02 |
| p1 | Action (SDK lane) | Update current position on map | FR-03-02 |
| d3 | Decision | Map still open? | FR-03-02 ("WHILE the map is open") |
| s7 | Action | Query POI and attraction data | FR-03-03 |
| d2 | Decision | Search result? | FR-03-03 (BE row: not-found, closed, system error) |
| s8 | Action | Show error message: unable to load data, try again | FR-03-03 (FE row: retry message) |
| s9 | Action | Show not-found message and similar names | FR-03-03 |
| s10 | Action | Show message: currently closed | FR-03-03 (BE row: closed attraction) |
| s11 | Action | Show location, status, wait time and walking time | FR-03-03 (see section 6 \#9) |
| g2 | Action (SDK lane) | Calculate walking route, time and distance on device | FR-03-04 (FE row: client-side SDK routing) |
| s12 | Action | Show route, walking time and distance | FR-03-04 |
| t2 | Time event | Periodic refresh while an attraction is viewed | FR-03-05 |
| w1 | Action | Get latest wait time and status | FR-03-05 |
| d4 | Decision | Attraction status? | FR-03-05 |
| w2 | Action | Show wait time and status Open | FR-03-05 |
| w3 | Action | Show status Closed | FR-03-05 |
| s13 | Action | Save wait time and status | FR-03-06 |
| s14 | Action | Confirm update to Staff | FR-03-06 (BE row: confirm successful saving) |

FR names as in W1 (`FunctionList`):

| FR | W1 name |
| :---- | :---- |
| FR-03-01 | View interactive park map and points of interest |
| FR-03-02 | View current visitor location on the park map |
| FR-03-03 | Search attractions and points of interest |
| FR-03-04 | Get walking directions to a selected destination |
| FR-03-05 | View real-time attraction wait time and operating status |
| FR-03-06 | Update attraction wait time and operating status |

## 3\. Branches and acceptance tests

Example values (Ferris Wheel, wait 25 min) are illustrative; replace with the real attraction names in the park data.

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[granted\] | the visitor opened the map and the phone's location setting is on | the app requests permission and the visitor allows it | the current position is read and shown on the map, and the position keeps updating while the map is open |
| d1 \[denied\] | the visitor opened the map | the visitor denies location permission | the map and POIs are still shown, there is no current-position marker, and no position updates run |
| d3 \[map open\] | permission was granted and the map is on screen | the map is still open | the position is read again and the marker moves |
| d3 \[map closed\] | permission was granted | the visitor leaves the map screen | position updates stop; the other flows are not affected |
| d2 \[data unavailable\] | the visitor searched "Ferris Wheel" | the data source returns an error | the visitor sees "unable to load data, try again" and the activity ends |
| d2 \[no match\] | no POI is named "Ferris Whel" | the visitor searches "Ferris Whel" | a not-found message and similar names (for example "Ferris Wheel") are shown; the visitor can search again |
| d2 \[match, attraction Closed\] | Ferris Wheel is in status Closed | the visitor searches "Ferris Wheel" | the visitor sees "currently closed"; no route is offered |
| d2 \[match, attraction Open\] | Ferris Wheel is Open with wait time 25 min | the visitor searches "Ferris Wheel" | location, status, wait time 25 min and walking time are shown; the visitor can ask for directions |
| d4 \[Open\] | the visitor is viewing Ferris Wheel, status Open, wait 25 min | the refresh time event fires | the latest wait time and status Open are shown |
| d4 \[Closed\] | Staff set Ferris Wheel to Closed after the visitor opened its page | the refresh time event fires | status Closed is shown instead of a wait time |
| Staff flow (no decision) | Ferris Wheel wait time is 25 min | Staff sets it to 40 min | the new value is saved, Staff sees the update confirmed, and visitors see 40 min at their next read |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Attraction | Open, Closed (read in `d2`, `d4`; written by Staff in `s13`) | \[NEEDS CLARIFICATION: add states from other diagrams, if any\] |
| Location permission | Granted, Denied (guards of `d1`) | Granted, Denied |

## 5\. Screens touched

Leave empty in Lesson 7\. Fill in Lesson 8 from the Screen List.

| Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: FR-03-02 only says WHILE location permission is available. No FR says what happens when permission is denied, or whether the app prompts the visitor to enable location. Add an IF clause to FR-03-02?\] | No | Map and POIs are shown without a position marker; no prompt. Branch `d1 [denied]` traces to FR-03-02 by its condition only. |
| 2 | \[NEEDS CLARIFICATION: the workbook has no business-rule list for F-03. Are there rules (for example what counts as "closed")?\] | No | None applied. |
| 3 | \[NEEDS CLARIFICATION: FR-03-02 and FR-03-05 give no refresh interval for position and wait time.\] | No | Loop and time event say only "while map open" and "periodic". |
| 4 | \[NEEDS CLARIFICATION: FR-03-04 requires a current location. What does the visitor get when the location is unavailable (permission denied or no GPS fix)?\] | Yes | No route is offered. Not drawn, because no FR defines the outcome. |
| 5 | \[NEEDS CLARIFICATION: what happens if map data cannot be loaded at open, or GPS or Beacon returns no position? FR-03-03 covers errors only for search.\] | No | Not drawn. |
| 6 | \[NEEDS CLARIFICATION: the lane "Staff & Admin" (workshop assignment) vs "operation staff" (FR-03-06) vs "Operation / Ride Staff" (sequence diagram). Which name is on the Context Diagram?\] | No | "Staff & Admin". |
| 7 | \[NEEDS CLARIFICATION: the diagram has about 40 nodes, above the guideline of about 25\. It holds three flows (map and location, search and route, Staff update). Split UC-03 in W0, or keep one use case?\] | No | Kept as one file as assigned; integrator decides. |
| 8 | \[NEEDS CLARIFICATION: FR-03-06 has an L3 row "enforce staff authorisation and validate allowed fields". What does Staff see when the update is rejected?\] | No | Not drawn: the EARS criterion of FR-03-06 has no IF clause. |
| 9 | \[NEEDS CLARIFICATION: FR-03-03 says "matching attractions and available details". Does that include wait time and walking time, as sequence step 20 shows?\] | No | Yes, traced to FR-03-03. |
| 10 | \[NEEDS CLARIFICATION: after "currently closed" or an error, can the visitor search again?\] | No | The activity ends; a new search is a new run of the use case. |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Parse check passed (Mermaid 12.2.0); visual check on mermaid.live pending |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1 (2), d2 (4), d3 (2), d4 (2) |  |  |
| No action has two incoming flows (merge used instead) | Pass: `m1` and `mloc` are merges; only `stop` nodes have several incoming flows |  |  |
| Every fork has a matching join | Fork `f1` has no join: its loop branch ends in the flow final `x1`; the other branch ends in activity finals |  |  |
| Every system action cites an FR | Pass (FR-03-01 to FR-03-06 from W1) |  |  |
| Every FR of this use case appears in the diagram | Pass (6 of 6\) |  |  |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Open: see section 6 \#6 |  |  |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Partly: alt fragments appear as `d2` and `d4`; FR IDs to be added to the sequence diagram |  |  |
| AI critic prompt run, findings resolved or logged as open questions | Pending |  |  |

&nbsp;