# W2 Costing and Roll-up: MySen Platform

Lesson 5 (block W2). Parameters applied:
- AI Leverage Factor: FE = 0.35, BE = 0.7, INT = 1.0.
- Verification Factor: 0.15 (precise EARS).
- Rates: Junior (FE) = 200,000 VND/h; Senior (BE, INT) = 300,000 VND/h.

## Cost per L3 work item

| L3 ID | Feature | Layer | Base h | AI-adj h | Verif h | Person-hours | Band | Cost (VND) | Effective Priority |
|---|---|---|---|---|---|---|---|---|---|
| FR-00-01-FE | F-00 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Must |
| FR-00-01-BE | F-00 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Must |
| FR-00-02-BE | F-00 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Must |
| FR-00-03-BE | F-00 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Must |
| FR-00-04-BE | F-00 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Must |
| FR-02-01-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Must |
| FR-02-01-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Must |
| FR-02-02-FE | F-02 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800,000 | Must |
| FR-02-02-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Must |
| FR-02-02-INT | F-02 | INT | 16 | 16.0 | 2.4 | 18.4 | Senior | 5,520,000 | Must |
| FR-02-03-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Must |
| FR-02-03-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Must |
| FR-03-01-FE | F-03 | FE | 16 | 5.6 | 2.4 | 8.0 | Junior | 1,600,000 | Must |
| FR-03-01-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Must |
| FR-04-01-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Must |
| FR-04-01-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Must |
| FR-04-02-FE | F-04 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800,000 | Must |
| FR-04-02-BE | F-04 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Must |
| FR-04-03-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Must |
| FR-04-03-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Must |
| FR-04-04-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Must |
| FR-04-04-BE | F-04 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Must |
| FR-04-05-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Should |
| FR-04-05-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Should |
| FR-05-01-FE | F-05 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800,000 | Should |
| FR-05-01-BE | F-05 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Should |
| FR-05-02-FE | F-05 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400,000 | Could |
| FR-05-02-BE | F-05 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1,020,000 | Could |
| FR-06-01-FE | F-06 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800,000 | Must |
| FR-06-01-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Must |
| FR-06-01-INT| F-06 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2,760,000 | Must |
| FR-06-02-FE | F-06 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800,000 | Could |
| FR-06-02-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2,040,000 | Could |

---

## Roll-up per feature

| Feature | Feature Name | L2 | L3 | Base h | Must p-h | Should p-h | Could p-h | In-scope p-h | In-scope cost (VND) | Must Share |
|---|---|---|---|---|---|---|---|---|---|---|
| F-00 | Platform & Authentication | 4 | 5 | 28 | 22.4 | 0 | 0 | 22.4 | 6,520,000 | 100% |
| F-02 | E-Ticketing & Wallet | 3 | 7 | 44 | 40.0 | 0 | 0 | 40.0 | 11,200,000 | 100% |
| F-03 | Interactive Map & Navigation | 1 | 2 | 24 | 14.8 | 0 | 0 | 14.8 | 3,640,000 | 100% |
| F-04 | Gamification & Loyalty | 5 | 10 | 52 | 30.4 | 5.4 | 0 | 35.8 | 9,920,000 | 85% |
| F-05 | Social & Partner Ecosystem | 2 | 4 | 24 | 0 | 10.8 | 5.4 | 16.2 | 4,260,000 | 0% |
| F-06 | Admin Operations & Analytics | 2 | 5 | 40 | 20.0 | 0 | 10.8 | 30.8 | 8,440,000 | 65% |
| **Project**| **Total MySen Platform** | **17** | **33** | **212** | **127.6** | **16.2** | **16.2** | **160.0** | **43,980,000** | **80%** |

---

## The Three Headline Figures

| Scenario | Scope | Person-hours | Cost (VND) | Meaning |
|---|---|---|---|---|
| Committed | Must only | 127.6 | 35,460,000 | Guaranteed core product delivery |
| Expected | Must + Should | 143.8 | 39,720,000 | Base contract figure presented to client |
| Ceiling | Must + Should + Could | 160.0 | 43,980,000 | Maximum scope including contingency pool |