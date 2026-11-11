# AI log: where AI suggested something wrong

Kept for the whole project. One entry each time an AI tool proposed something the team did not accept, with the decision and where it was recorded.

| # | Lesson | What AI suggested | Why it was wrong for MySen Platform | What the team did |
|---|---|---|---|---|
| 1 | L5 (W0) | Add an Augmented Reality (AR) 3D camera viewer for park quests | High hardware requirement and development complexity out of scope for Release 1 | Recorded in W0 Table 3 (Won't-have) with explicit exclusion reason |
| 2 | L5 (W1) | Estimate "RBAC Middleware & Route Guards" as zero work because it scores zero function points | It is an NC (Non-Countable) row that contains essential security logic for Admin and Partner access | Kept as FR-00-02-BE, sized S (4h), with its own L3 work item |
| 3 | L7 | Draw the activity diagram with separate swimlanes for Frontend, Backend, and Database | In DBIZ2, system activity diagrams treat the platform as a single system lane; inner architecture details belong in sequence diagrams | Redrawn with system actor swimlanes (e.g., Visitor, MySen Platform, Payment Gateway) |
| 4 | L8 | Generated UI Wireframe included an automatic cash refund button on the Ticket Wallet | Automatic refunds pose financial fraud risks and require manual park management verification | Removed from `S-F02-03` wireframe; logged as manual support process in business rules |