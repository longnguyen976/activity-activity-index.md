# Activity Diagram: AD-UC-07 Register Indie Artist Performance Schedule

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-07 |
| Use case | UC-07 Register Indie Artist Performance Schedule |
| Feature | F-05 Partner Platform & Event Management |
| Primary actor | Indie Artists & Creators (secondary actors: Staff & Admin, who approve events; Visitor / GenZ Customer, who views the calendar) |
| Lanes | Indie Artists & Creators · Staff & Admin · MySen App Platform · Visitor / GenZ Customer |
| Inputs (L3-L5) | Context Diagram v<n> · Use Case Diagram v<n> · Sequence: none drawn for UC-07 yet · W1 rows FR-05-04 to FR-05-06 |
| Trigger | An artist submits a performance schedule (artist flow). A visitor opens the Events tab (visitor flow). |
| Preconditions | The artist has an account and is signed in. Stages are known to the system. |
| Success end state | The event is APPROVED, published to the calendar and the artist is notified. The visitor sees the approved events for the chosen date and category. |
| Failure end state(s) | Clash: the submission is rejected with the conflict reason. Staff reject: the event is REJECTED and the artist is notified. No events match: the visitor sees "no events found". |
| FRs covered | FR-05-04, FR-05-05, FR-05-06 |
| Business rules applied | none found in the workbook \[NEEDS CLARIFICATION: see section 6 \#7\] |
| Version / status | v0.1 · Draft |
| Author | Trần Hương Ly (owner of F-05) |

## 1\. Diagram

&nbsp;

```mermaid
swimlane-beta TB
subgraph ART["Indie Artists & Creators"]
start@{ shape: sm-circ }
m1{" "}
a1("Submit performance schedule: date, time and stage")
d3{"Resubmit<br>schedule?"}
stop1@{ shape: fr-circ }
end
subgraph STF["Staff & Admin"]
o1("Review pending event listing")
d5{"Approve<br>event?"}
end
subgraph SYS["MySen App Platform"]
s1("Create event listing in PENDING state (FR-05-04)")
d1{"Clash with an approved<br>event at same stage<br>and time?"}
mc{" "}
s2("Reject submission and notify creator with conflict reason (FR-05-05)")
s3("Queue listing for admin approval and notify Staff (FR-05-05)")
d2{"Still clash-free<br>at approval?"}
s4("Set event APPROVED and publish to calendar (FR-05-05)")
s5("Notify creator: event approved (FR-05-05)")
s6("Set event REJECTED with reason and notify creator (FR-05-05)")
s7("Query approved events by date and category (FR-05-06)")
d4{"Matching<br>events?"}
s8("Show event list (FR-05-06)")
s9("Show message: no events found (FR-05-06)")
stop2@{ shape: fr-circ }
stop3@{ shape: fr-circ }
stop4@{ shape: fr-circ }
end
subgraph VIS["Visitor / GenZ Customer"]
start2@{ shape: sm-circ }
v1("Open Events tab and pick date and category")
end
start --> m1 --> a1 --> s1 --> d1
d1 -->|"[clash]"| mc --> s2 --> d3
d1 -->|"[no clash]"| s3 --> o1 --> d5
d3 -->|"[resubmit]"| m1
d3 -->|"[give up]"| stop1
d5 -->|"[rejected]"| s6 --> stop2
d5 -->|"[approved]"| d2
d2 -->|"[clash found]"| mc
d2 -->|"[clash-free]"| s4 --> s5 --> stop3
start2 --> v1 --> s7 --> d4
d4 -->|"[events found]"| s8 --> stop4
d4 -->|"[none]"| s9 --> stop4
```

&nbsp;

Reading notes:

- **Two flows, two initial nodes.** (1) Artist flow from start: submit, clash check, approval. (2) Visitor flow from start2: view the calendar. They are **not connected by a control flow**. They depend on each other only through stored data: s4 sets an event to APPROVED; s7 reads only APPROVED events.  
- **Merge m1** receives the start of the artist flow and the "resubmit" loop. **Merge mc** receives two flows: a clash found at submission (d1) and a clash found again at approval (d2), so s2 has one incoming flow.  
- **Second clash check d2.** FR-05-05 compares a submission only with approved events. Two PENDING listings for the same slot could both be approved, so the clash is checked again when Staff approve.  
- **No fork, no join.** All branches end in activity finals.  
- **Lane mapping.** Backend, database and notification service are inside "MySen App Platform". Admin is "Staff & Admin" (see section 6 \#4).

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| a1 | Action (Artist lane) | Submit performance schedule: date, time and stage | FR-05-04 (FE row: schedule form) |
| s1 | Action | Create event listing in PENDING state | FR-05-04 |
| d1 | Decision | Clash with an approved event at same stage and time? | FR-05-05 (condition of the EARS sentence) |
| s2 | Action | Reject submission and notify creator with conflict reason | FR-05-05 |
| d3 | Decision | Resubmit schedule? | FR-05-04 (see section 6 \#5) |
| s3 | Action | Queue listing for admin approval and notify Staff | FR-05-05 (BE row: admin approval workflow; notice to Staff see section 6 \#3) |
| o1 | Action (Staff lane) | Review pending event listing | FR-05-05 |
| d5 | Decision | Approve event? | FR-05-05 (BE row: admin approval workflow) |
| d2 | Decision | Still clash-free at approval? | FR-05-05 (see section 6 \#1) |
| s4 | Action | Set event APPROVED and publish to calendar | FR-05-05 |
| s5 | Action | Notify creator: event approved | FR-05-05 (see section 6 \#2) |
| s6 | Action | Set event REJECTED with reason and notify creator | FR-05-05 (see section 6 \#2) |
| v1 | Action (Visitor lane) | Open Events tab and pick date and category | FR-05-06 (FE row: event calendar with filters) |
| s7 | Action | Query approved events by date and category | FR-05-06 (BE row: query approved events) |
| d4 | Decision | Matching events? | FR-05-06 |
| s8 | Action | Show event list | FR-05-06 |
| s9 | Action | Show message: no events found | FR-05-06 (see section 6 \#6) |

FR names as in W1 (FunctionList):

| FR | W1 name |
| :---- | :---- |
| FR-05-04 | Publish Indie artist performance schedule |
| FR-05-05 | Approve event before publishing |
| FR-05-06 | View public event calendar |

## 3\. Branches and acceptance tests

Example values (Stage A, dates) are illustrative; replace them with the real stages and dates.

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[clash\] | an approved event at Stage A, 19:00 to 20:00 | an artist submits a show at Stage A, 19:30 to 20:30 | the submission is rejected and the artist sees the conflict reason |
| d1 \[no clash\] | Stage A is free from 21:00 to 22:00 | an artist submits a show for that slot | the listing stays PENDING, is queued for approval and Staff are notified |
| d3 \[resubmit\] | a rejected submission with a conflict reason | the artist changes the time and submits again | the flow returns through merge m1 and the new submission is checked again |
| d3 \[give up\] | a rejected submission | the artist does not resubmit | the submission stays closed and no event is shown to visitors |
| d5 \[approved\] | a PENDING, clash-free listing | Staff approve it | the clash is checked again (d2) and, if clear, the event is APPROVED |
| d5 \[rejected\] | a PENDING listing | Staff reject it with a reason | the event becomes REJECTED and the artist is notified; visitors never see it |
| d2 \[clash found\] | another event for the same stage and time was approved while this one waited | Staff approve this one | the approval is refused and the artist gets the conflict reason (same path as d1 \[clash\]) |
| d2 \[clash-free\] | no other event was approved for that slot | Staff approve | the event is APPROVED, published and the artist is notified |
| d4 \[events found\] | two approved events on 15 Nov, category Music | a visitor filters 15 Nov and Music | both events are listed |
| d4 \[none\] | no approved events on 16 Nov | a visitor filters 16 Nov | a message says no events were found |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Event listing | PENDING, APPROVED, REJECTED (set in s1, s4, s6, s2) | \[NEEDS CLARIFICATION: add states from other diagrams, if any\] |

## 5\. Screens touched

Leave empty in Lesson 7\. Fill in Lesson 8 from the Screen List.

| Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: FR-05-05 compares a submission only with approved events, so two PENDING listings could both be approved. Is the second clash check d2 at approval accepted?\] | No | d2 drawn; traced to FR-05-05. |
| 2 | \[NEEDS CLARIFICATION: No FR says the artist is told when the event is approved (s5) or rejected by Staff (s6). Add a sentence to FR-05-05?\] | No | Drawn; traced to FR-05-05. |
| 3 | \[NEEDS CLARIFICATION: FR-05-05 says the system rejects a clash; UC-07 says an admin approves. Does Staff get a notice when a listing is queued (s3)?\] | No | Both drawn: the system rejects clashes first, then Staff decide; Staff are notified. |
| 4 | \[NEEDS CLARIFICATION: Lane "Staff & Admin" (Context Diagram) vs "Admin" (FR-05-05). The F-05 lane list given for this task has no staff lane, but UC-07 needs one.\] | No | "Staff & Admin". |
| 5 | \[NEEDS CLARIFICATION: Can a rejected artist resubmit, and how often (d3)?\] | No | One resubmit loop through m1; no limit. |
| 6 | \[NEEDS CLARIFICATION: Does the visitor see only future events, and is a past approved event still listed (d4)?\] | No | All approved events matching the filter are shown. |
| 7 | \[NEEDS CLARIFICATION: The workbook has no business-rule list for F-05 (for example a buffer time between two shows on one stage).\] | No | None applied. |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Parse check passed (Mermaid 11.16.0, mermaid-cli); visual check on mermaid.live pending |  | 2026-10-26 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1 (2), d2 (2), d3 (2), d4 (2), d5 (2) |  | 2026-10-26 |
| No action has two incoming flows (merge used instead) | Pass: m1 and mc are merges; only stop nodes have several incoming flows |  | 2026-10-26 |
| Every fork has a matching join | Pass: no fork used |  | 2026-10-26 |
| Every system action cites an FR | Pass (FR-05-04 to FR-05-06 from W1) |  | 2026-10-26 |
| Every FR of this use case appears in the diagram | Pass (3 of 3\) |  | 2026-10-26 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Open: see section 6 \#4 |  | 2026-10-26 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Not applicable yet: no sequence diagram for UC-07 |  | 2026-10-26 |
| AI critic prompt run, findings resolved or logged as open questions | Run by the same AI that drafted the diagram; findings logged as section 6 \#1 to \#3. Peer team to repeat independently |  | 2026-10-26 |

&nbsp;