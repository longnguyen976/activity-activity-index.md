# W6 Cash Flow and Investment Decision: MySen Platform

Lesson 6 (Capital Budgeting). Evaluates financial viability across 3 scenarios over a 5-year evaluation period.

## 1. Valuation Assumptions

| Assumption | Value |
|---|---|
| Discount Rate (Hurdle Rate) | 12% per year |
| Evaluation Horizon | 5 years |
| Annual Maintenance & Operational Cost | 15% of initial build cost per year |
| Financial Contingency | 10% of Must cost = 3,546,000 VND |
| Steady-State Annual Benefit | Must: 22,000,000 VND | Should: 8,000,000 VND | Could: 3,000,000 VND |
| Benefit Ramp-up Rate | Year 1: 40% · Year 2: 70% · Year 3-5: 100% |

---

## 2. Cash Flow Projections

### Scenario A: Pessimistic (Must Only)
* Initial Investment (Year 0): 35,460,000 + 3,546,000 = **39,006,000 VND**
* Annual Running Cost: **5,319,000 VND**

| Line Item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 8,800,000 | 15,400,000 | 22,000,000 | 22,000,000 | 22,000,000 |
| Running Cost | 0 | -5,319,000 | -5,319,000 | -5,319,000 | -5,319,000 | -5,319,000 |
| Investment | -39,006,000 | 0 | 0 | 0 | 0 | 0 |
| **Net Cash Flow** | **-39,006,000** | **3,481,000** | **10,081,000** | **16,681,000** | **16,681,000** | **16,681,000** |
| **Cumulative NCF**| **-39,006,000** | **-35,525,000**| **-25,444,000**| **-8,763,000** | **7,918,000** | **24,599,000** |
| **PV of NCF** | **-39,006,000** | **3,108,036** | **8,036,512** | **11,873,205** | **10,599,290** | **9,463,652** |

---

### Scenario B: Base (Must + Should)
* Initial Investment (Year 0): 39,720,000 + 3,546,000 = **43,266,000 VND**
* Annual Running Cost: **5,958,000 VND**

| Line Item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 12,000,000 | 21,000,000 | 30,000,000 | 30,000,000 | 30,000,000 |
| Running Cost | 0 | -5,958,000 | -5,958,000 | -5,958,000 | -5,958,000 | -5,958,000 |
| Investment | -43,266,000 | 0 | 0 | 0 | 0 | 0 |
| **Net Cash Flow** | **-43,266,000** | **6,042,000** | **15,042,000** | **24,042,000** | **24,042,000** | **24,042,000** |
| **Cumulative NCF**| **-43,266,000** | **-37,224,000**| **-22,182,000**| **1,860,000** | **25,902,000** | **49,944,000** |
| **PV of NCF** | **-43,266,000** | **5,394,643** | **11,991,390** | **17,112,654** | **15,279,155** | **13,642,103** |

---

### Scenario C: Optimistic (Full Scope)
* Initial Investment (Year 0): 43,980,000 + 3,546,000 = **47,526,000 VND**
* Annual Running Cost: **6,597,000 VND**

| Line Item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 13,200,000 | 23,100,000 | 33,000,000 | 33,000,000 | 33,000,000 |
| Running Cost | 0 | -6,597,000 | -6,597,000 | -6,597,000 | -6,597,000 | -6,597,000 |
| Investment | -47,526,000 | 0 | 0 | 0 | 0 | 0 |
| **Net Cash Flow** | **-47,526,000** | **6,603,000** | **16,503,000** | **26,403,000** | **26,403,000** | **26,403,000** |
| **Cumulative NCF**| **-47,526,000** | **-40,923,000**| **-24,420,000**| **1,983,000** | **28,386,000** | **54,789,000** |
| **PV of NCF** | **-47,526,000** | **5,895,536** | **13,156,161** | **18,793,028** | **16,779,489** | **14,981,687** |

---

## 3. Financial Summary & Decision Signal

| Metric | Pessimistic (Must) | Base (Must + Should) | Optimistic (Full Scope) |
|---|---|---|---|
| Year 0 Investment | 39,006,000 VND | 43,266,000 VND | 47,526,000 VND |
| Steady-State Annual Benefit | 22,000,000 VND | 30,000,000 VND | 33,000,000 VND |
| **NPV (at 12%)** | **4,074,695 VND** | **20,153,945 VND** | **22,079,901 VND** |
| **IRR** | **15.6%** | **24.5%** | **23.9%** |
| **Payback Period** | **3.52 years** | **2.92 years** | **2.92 years** |
| **Profitability Index (PI)** | **1.10** | **1.47** | **1.46** |
| **Verdict** | **ACCEPT** | **RECOMMENDED (BASE)** | **ACCEPT** |

### Executive Conclusion:
* The **Base Scenario (Must + Should)** delivers the highest Internal Rate of Return (**24.5%**) and strong NPV (**20,153,945 VND**) with a payback period under 3 years (**2.92 years**).
* This validates the commitment of all Must and Should features for the initial release of MySen Platform.