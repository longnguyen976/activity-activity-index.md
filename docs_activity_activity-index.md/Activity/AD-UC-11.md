# Activity Diagram: AD-UC-11 Post & Browse Social Gallery

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-11 |
| Use case | UC-11 Post & Browse Social Gallery |
| Feature | F-05 Partner Platform & Event Management |
| Primary actor | Visitor / GenZ Customer (secondary actor: Staff & Admin, who moderate posts) |
| Lanes | Visitor / GenZ Customer · Staff & Admin · MySen App Platform · Cloud Storage / CDN |
| Inputs (L3-L5) | Context Diagram v<n> · Use Case Diagram v<n> · Sequence: S-11a, S-11b, S-11c (UC-11 sequence diagrams) · W1 rows FR-05-01 to FR-05-03 |
| Trigger | The visitor taps Post (post flow), reports a post (report flow) or opens the Social Gallery tab (browse flow). |
| Preconditions | The visitor is signed in. The photo storage on Cloud Storage / CDN is reachable. |
| Success end state | A clean post is published to the feed. A flagged or reported post is held hidden until Staff decide. The visitor sees published posts, newest first. |
| Failure end state(s) | Upload failed: no post is created and the visitor can retry. Post violates the rules: removed and the author is notified. No published posts: the visitor sees "gallery is empty". |
| FRs covered | FR-05-01, FR-05-02, FR-05-03 |
| Business rules applied | none found in the workbook \[NEEDS CLARIFICATION: see section 6 \#7\] |
| Version / status | v0.1 · Draft |
| Author | Trần Hương Ly (owner of F-05) |

## 1\. Diagram

&nbsp;

```mermaid
swimlane-beta TB
subgraph VIS["Visitor / GenZ Customer"]
start@{ shape: sm-circ }
m1{" "}
v1("Select photo, write caption, tag location and tap Post")
start2@{ shape: sm-circ }
v2("Report a post as violating")
start3@{ shape: sm-circ }
v3("Open Social Gallery tab")
d6{"Reached end<br>of the page?"}
end
subgraph STF["Staff & Admin"]
o1("Review post, photo and reports in the queue")
d4{"Post violates<br>the rules?"}
end
subgraph SYS["MySen App Platform"]
s1("Send photo to storage (FR-05-01)")
d1{"Upload<br>result?"}
s3("Show upload error, no post created (FR-05-01)")
s4("Save post with user_id and timestamp (FR-05-01)")
d2{"Automated filter<br>result?"}
s6("Publish post to the public feed (FR-05-01)")
s7("Hold post hidden and add to moderation queue (FR-05-02)")
s8("Hide reported post immediately and add to queue (FR-05-02)")
mq{" "}
s9("Remove post and notify author (FR-05-02)")
s10("Restore post to the public feed (FR-05-02)")
mp{" "}
s11("Query published posts, newest first, one page (FR-05-03)")
d3{"Published<br>posts found?"}
s13("Show photo grid (FR-05-03)")
s14("Show message: gallery is empty (FR-05-03)")
stop1@{ shape: fr-circ }
stop2@{ shape: fr-circ }
stop3@{ shape: fr-circ }
stop4@{ shape: fr-circ }
stop5@{ shape: fr-circ }
end
subgraph CDN["Cloud Storage / CDN"]
c1("Store photo and return its URL (FR-05-01)")
c2("Serve photo thumbnails by URL (FR-05-01)")
end
start --> m1 --> v1 --> s1 --> c1 --> d1
d1 -->|"[upload failed]"| s3 --> m1
d1 -->|"[stored]"| s4 --> d2
d2 -->|"[not flagged]"| s6 --> stop1
d2 -->|"[flagged]"| s7 --> mq
start2 --> v2 --> s8 --> mq
mq --> o1 --> d4
d4 -->|"[violates]"| s9 --> stop2
d4 -->|"[no violation]"| s10 --> stop3
start3 --> v3 --> mp --> s11 --> d3
d3 -->|"[none]"| s14 --> stop4
d3 -->|"[found]"| s13 --> c2 --> d6
d6 -->|"[reached end]"| mp
d6 -->|"[stops scrolling]"| stop5
```

&nbsp;

Reading notes:

- **Three flows, three initial nodes.** (1) Post a photo from start. (2) Report a post from start2. (3) Browse the feed from start3. The flows are **not connected by a control flow**. They depend on each other only through stored posts: s6 publishes a post; s11 reads published posts; s8 hides one.  
- **Merge m1** receives the start of the post flow and the retry after an upload error. **Merge mq** receives two flows into one moderation queue: a post flagged by the automated filter and a post hidden after a report. **Merge mp** receives the start of the browse flow and the "next page" loop.  
- **Publish only after the filter.** s6 is reached only when d2 answers "not flagged". A flagged post is hidden at once (s7), as FR-05-02 requires.  
- **No fork, no join.** All branches end in activity finals.  
- **Lane mapping.** Backend, database and the auto-filter are inside "MySen App Platform" (black-box view). The moderator is "Staff & Admin" (see section 6 \#4).

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| v1 | Action (Visitor lane) | Select photo, write caption, tag location and tap Post | FR-05-01 (FE row: post screen) |
| s1 | Action | Send photo to storage | FR-05-01 |
| c1 | Action (CDN lane) | Store photo and return its URL | FR-05-01 (INT row: Cloud Storage/CDN) |
| d1 | Decision | Upload result? | FR-05-01 (see section 6 \#1) |
| s3 | Action | Show upload error, no post created | FR-05-01 (see section 6 \#1) |
| s4 | Action | Save post with user\_id and timestamp | FR-05-01 (BE row: save post) |
| d2 | Decision | Automated filter result? | FR-05-02 (condition "flagged by the automated filter") |
| s6 | Action | Publish post to the public feed | FR-05-01 |
| s7 | Action | Hold post hidden and add to moderation queue | FR-05-02 (BE row: moderation queue \+ rule engine) |
| v2 | Action (Visitor lane) | Report a post as violating | FR-05-02 (FE row: report button) |
| s8 | Action | Hide reported post immediately and add to queue | FR-05-02 |
| o1 | Action (Staff lane) | Review post, photo and reports in the queue | FR-05-02 ("manual review") |
| d4 | Decision | Post violates the rules? | FR-05-02 (see section 6 \#2) |
| s9 | Action | Remove post and notify author | FR-05-02 (see section 6 \#2) |
| s10 | Action | Restore post to the public feed | FR-05-02 (see section 6 \#2) |
| v3 | Action (Visitor lane) | Open Social Gallery tab | FR-05-03 (FE row: photo feed) |
| s11 | Action | Query published posts, newest first, one page | FR-05-03 (BE row: query, pagination) |
| d3 | Decision | Published posts found? | FR-05-03 |
| s13 | Action | Show photo grid | FR-05-03 |
| c2 | Action (CDN lane) | Serve photo thumbnails by URL | FR-05-01 (INT row; see section 6 \#5) |
| s14 | Action | Show message: gallery is empty | FR-05-03 (see section 6 \#6) |
| d6 | Decision | Reached end of the page? | FR-05-03 (FE row: infinite scroll) |

FR names as in W1 (FunctionList):

| FR | W1 name |
| :---- | :---- |
| FR-05-01 | Post a photo to the Social Gallery |
| FR-05-02 | Moderate Social Gallery content |
| FR-05-03 | View the Social Gallery feed |

## 3\. Branches and acceptance tests

Example values (banned keyword, empty feed) are illustrative; replace them with the real moderation rules.

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[stored\] | a visitor has a photo and a caption | the visitor taps Post and the CDN stores the photo | the post is saved with user\_id and timestamp |
| d1 \[upload failed\] | the CDN cannot be reached | the visitor taps Post | an error is shown, no post is created and the visitor can try again through merge m1 |
| d2 \[not flagged\] | a clean photo and caption | the post is saved | the post is published to the public feed |
| d2 \[flagged\] | a caption with a banned keyword | the post is saved | the post is held hidden and added to the moderation queue; it is never shown first |
| Report path (no decision) | a published post | a visitor reports it | the post is hidden at once and joins the queue before any moderator looks |
| d4 \[violates\] | a queued or reported post | Staff confirm the violation | the post is removed and the author is notified |
| d4 \[no violation\] | a reported post that follows the rules | Staff clear it | the post is restored to the public feed |
| d3 \[found\] | published posts exist | a visitor opens the Social Gallery tab | the first page shows the newest posts first and the photos load from the CDN |
| d3 \[none\] | no published posts | a visitor opens the tab | a message says the gallery is empty |
| d6 \[reached end\] | the first page of posts is shown | the visitor scrolls to the end of the page | the next page is queried through merge mp |
| d6 \[stops scrolling\] | a page of posts is shown | the visitor stops scrolling | browsing ends and no more queries are made |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Post | Published, Hidden pending review, Removed (set in s6, s7, s8, s9, s10) | \[NEEDS CLARIFICATION: add states from other diagrams, if any\] |

## 5\. Screens touched

Leave empty in Lesson 7\. Fill in Lesson 8 from the Screen List.

| Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: FR-05-01 has no failure path for a failed upload. Add an IF clause?\] | No | Error and retry drawn (s3, m1); traced to FR-05-01. |
| 2 | \[NEEDS CLARIFICATION: FR-05-02 covers hiding a post pending manual review, not removing it, restoring it or telling the author (s9, s10).\] | No | Drawn; traced to FR-05-02. |
| 3 | \[NEEDS CLARIFICATION: FR-05-01 and FR-05-03 are Should and FR-05-02 is Must in W1. If only Must is built, a moderation queue exists but no posting.\] | No | All three FRs drawn as written. |
| 4 | \[NEEDS CLARIFICATION: Lane "Staff & Admin" (Context Diagram) vs "moderator" (sequence diagram S-11b). The F-05 lane list given for this task has no staff lane, but moderation needs one.\] | No | "Staff & Admin". |
| 5 | \[NEEDS CLARIFICATION: Does the CDN lane serve photos when browsing (c2)? FR-05-01-INT covers only storing the photo.\] | No | Drawn and traced to FR-05-01 (INT row). |
| 6 | \[NEEDS CLARIFICATION: FR-05-03 gives no empty-feed behaviour (s14).\] | No | Message drawn; traced to FR-05-03. |
| 7 | \[NEEDS CLARIFICATION: The workbook has no business-rule list for F-05 (for example the banned keywords, page size).\] | No | None applied. |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Parse check passed (Mermaid 11.16.0, mermaid-cli); visual check on mermaid.live pending |  | 2026-10-26 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1 (2), d2 (2), d3 (2), d4 (2), d6 (2) |  | 2026-10-26 |
| No action has two incoming flows (merge used instead) | Pass: m1, mq and mp are merges; only stop nodes have several incoming flows |  | 2026-10-26 |
| Every fork has a matching join | Pass: no fork used |  | 2026-10-26 |
| Every system action cites an FR | Pass (FR-05-01 to FR-05-03 from W1) |  | 2026-10-26 |
| Every FR of this use case appears in the diagram | Pass (3 of 3\) |  | 2026-10-26 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Open: see section 6 \#4 |  | 2026-10-26 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Partly: S-11a, S-11b, S-11c use the same lanes; FR IDs to be added to the sequence diagrams |  | 2026-10-26 |
| AI critic prompt run, findings resolved or logged as open questions | Run by the same AI that drafted the diagram; findings logged as section 6 \#1 to \#3 and \#5. Peer team to repeat independently |  | 2026-10-26 |

&nbsp;