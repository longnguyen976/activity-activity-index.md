# Activity Diagram: AD-UC-06 Manage Rental Space & Sponsorship Deals

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-06 |
| Use case | UC-06 Manage Rental Space & Sponsorship Deals |
| Feature | F-05 Partner Platform & Event Management |
| Primary actor | F\&B / Retail Partners (secondary actor: Staff & Admin, who maintains the space list and approves or rejects requests) |
| Lanes | F\&B / Retail Partners · Staff & Admin · MySen App Platform |
| Inputs (L3-L5) | Context Diagram v<n> · Use Case Diagram v<n> · Sequence: none drawn for UC-06 yet · W1 rows FR-05-07 and FR-05-08 |
| Trigger | A partner submits a rental-space request (partner flow). Ops staff create or update a rental-space listing (Staff flow). |
| Preconditions | The partner has an account and is signed in. At least one rental space is listed. |
| Success end state | The request is APPROVED, the space is marked booked and the partner is notified. Staff flow: the listing is stored. |
| Failure end state(s) | Space not available: no request is created. Request rejected: the partner is notified with the reason and may resubmit. |
| FRs covered | FR-05-07, FR-05-08 |
| Business rules applied | none found in the workbook \[NEEDS CLARIFICATION: see section 6 \#6\] |
| Version / status | v0.1 · Draft |
| Author | Trần Hương Ly (owner of F-05) |

## 1\. Diagram

```mermaid
swimlane-beta TB
subgraph PAR["F&B / Retail Partners"]
start2@{ shape: sm-circ }
m1{" "}
p1("Choose a space and submit rental request")
d3{"Resubmit<br>request?"}
stop3@{ shape: fr-circ }
end
subgraph STF["Staff & Admin"]
start@{ shape: sm-circ }
o1("Create or update rental-space listing")
o2("Review pending request")
d2{"Approve<br>request?"}
end
subgraph SYS["MySen App Platform"]
s1("Store space details: location, size, price, availability (FR-05-07)")
d1{"Space<br>available?"}
s3("Show message: space not available (FR-05-08)")
s4("Create request in PENDING state (FR-05-08)")
s5("Notify Operations team (FR-05-08)")
s6("Set request APPROVED and mark space booked (FR-05-07)")
s7("Notify partner: request approved (FR-05-08)")
s8("Set request REJECTED with reason (FR-05-08)")
s9("Notify partner: request rejected with reason (FR-05-08)")
stop1@{ shape: fr-circ }
stop2@{ shape: fr-circ }
end
start --> o1 --> s1 --> stop1
start2 --> m1 --> p1 --> d1
d1 -->|"[available]"| s4 --> s5 --> o2 --> d2
d1 -->|"[not available]"| s3 --> m1
d2 -->|"[approved]"| s6 --> s7 --> stop2
d2 -->|"[rejected]"| s8 --> s9 --> d3
d3 -->|"[resubmit]"| m1
d3 -->|"[give up]"| stop3
```

&nbsp;

Reading notes:

- **Two flows, two initial nodes.** (1) Staff flow from start: Ops staff create or update a rental-space listing. (2) Partner flow from start2: request, review, decision. The two flows are **not connected by a control flow**. They depend on each other only through stored data: s1 writes the space and its availability; d1 reads it.  
- **Merge m1** receives three flows: the start of the partner flow, "space not available", and "resubmit". The partner never goes back straight to the request action.  
- **No fork, no join.** All branches end in activity finals (stop1, stop2, stop3).  
- **Lane mapping.** Backend, database and notification service are inside "MySen App Platform" (black-box view). Ops staff are "Staff & Admin" (see section 6 \#4).

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| o1 | Action (Staff lane) | Create or update rental-space listing | FR-05-07 (FE row: admin screen) |
| s1 | Action | Store space details: location, size, price, availability | FR-05-07 (BE row: CRUD rental-space data) |
| p1 | Action (Partner lane) | Choose a space and submit rental request | FR-05-08 (FE row: request form) |
| d1 | Decision | Space available? | FR-05-07 (availability status) |
| s3 | Action | Show message: space not available | FR-05-08 (see section 6 \#2) |
| s4 | Action | Create request in PENDING state | FR-05-08 |
| s5 | Action | Notify Operations team | FR-05-08 |
| o2 | Action (Staff lane) | Review pending request | FR-05-08 (see section 6 \#3) |
| d2 | Decision | Approve request? | FR-05-08 (BE row: update status) |
| s6 | Action | Set request APPROVED and mark space booked | FR-05-08 and FR-05-07 (availability status) |
| s7 | Action | Notify partner: request approved | FR-05-08 (see section 6 \#1) |
| s8 | Action | Set request REJECTED with reason | FR-05-08 (BE row: update status) |
| s9 | Action | Notify partner: request rejected with reason | FR-05-08 (see section 6 \#1) |
| d3 | Decision | Resubmit request? | FR-05-08 (see section 6 \#5) |

FR names as in W1 (FunctionList):

| FR | W1 name |
| :---- | :---- |
| FR-05-07 | Manage rental spaces |
| FR-05-08 | Partner submits a rental-space request |

## 3\. Branches and acceptance tests

Example values are illustrative; replace them with the real spaces and prices in the park data.

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[available\] | a space is listed as available and a partner is signed in | the partner submits a rental request for it | the request is created in PENDING state and the Operations team is notified |
| d1 \[not available\] | the space was marked booked by another approval | the partner submits a request for it | no request is created; the partner sees that the space is not available and returns to the request form |
| d2 \[approved\] | a PENDING request exists and Ops staff opened it | Ops staff approve it | the request becomes APPROVED, the space is marked booked and the partner is notified |
| d2 \[rejected\] | a PENDING request exists | Ops staff reject it and enter a reason | the request becomes REJECTED with the reason and the partner is notified with it |
| d3 \[resubmit\] | a request was rejected and the partner read the reason | the partner edits and submits again | the flow returns through merge m1 and a new PENDING request is created |
| d3 \[give up\] | a request was rejected | the partner does not resubmit | the request stays REJECTED; the space stays available for other partners |
| Staff flow (no decision) | a rental space exists with price 5,000,000 VND | Ops staff change the price to 6,000,000 VND | the new details are saved and shown to partners at their next read |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Rental space | Available, Booked (read in d1; set to Booked in s6) | \[NEEDS CLARIFICATION: add states from other diagrams, if any\] |
| Rental request | PENDING, APPROVED, REJECTED (set in s4, s6, s8) | PENDING, APPROVED, REJECTED |

## 5\. Screens touched

Leave empty in Lesson 7\. Fill in Lesson 8 from the Screen List.

| Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: FR-05-08 only says the system notifies the Operations team. No FR says the partner is told the result (s7, s9). Add a sentence to FR-05-08?\] | No | Notifications drawn and traced to FR-05-08. |
| 2 | \[NEEDS CLARIFICATION: FR-05-08 has no failure path for a space that is no longer available (s3).\] | No | Message drawn; traced to FR-05-08 as the nearest FR. |
| 3 | \[NEEDS CLARIFICATION: Approve and reject by Ops staff have no EARS sentence; only "update status" in the BE row of FR-05-08.\] | No | o2 and d2 traced to FR-05-08. |
| 4 | \[NEEDS CLARIFICATION: Lane "Staff & Admin" (Context Diagram) vs "Ops staff" (FR-05-07 and FR-05-08). Which name is on the Context Diagram? The F-05 lane list given for this task has no staff lane, but UC-06 needs one.\] | No | "Staff & Admin". |
| 5 | \[NEEDS CLARIFICATION: Can a rejected partner resubmit, and how often (d3)? The workbook gives no rule.\] | No | One resubmit loop drawn through m1; no limit. |
| 6 | \[NEEDS CLARIFICATION: The workbook has no business-rule list for F-05 (for example who may rent which space, sponsorship deals in the use case title).\] | No | None applied. Sponsorship has no FR and is not drawn. |
| 7 | \[NEEDS CLARIFICATION: Two partners may request the same space at the same time; the space is booked only on approval (s6). Is a hold period needed?\] | No | Not drawn; Ops decides which request to approve. |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Parse check passed (Mermaid 11.16.0, mermaid-cli); visual check on mermaid.live pending |  | 2026-10-26 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1 (2), d2 (2), d3 (2) |  | 2026-10-26 |
| No action has two incoming flows (merge used instead) | Pass: m1 is a merge; only stop nodes have several incoming flows |  | 2026-10-26 |
| Every fork has a matching join | Pass: no fork used |  | 2026-10-26 |
| Every system action cites an FR | Pass (FR-05-07 and FR-05-08 from W1) |  | 2026-10-26 |
| Every FR of this use case appears in the diagram | Pass (2 of 2\) |  | 2026-10-26 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Open: see section 6 \#4 |  | 2026-10-26 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Not applicable yet: no sequence diagram for UC-06 |  | 2026-10-26 |
| AI critic prompt run, findings resolved or logged as open questions | Run by the same AI that drafted the diagram; findings logged as section 6 \#1 to \#3 and \#7. Peer team to repeat independently |  | 2026-10-26 |

&nbsp;