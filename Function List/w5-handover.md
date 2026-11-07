# W5 Baseline and handover: Vehicle Passport (TASCO Mobility)

The baseline every later step refers to.

## 1. Three scenarios

| Scenario | Person-hours | Cost (VND) | Role in the sensitivity analysis |
|---|---|---|---|
| Pessimistic — Must only | 168.4 | 48.520.000 | Lowest investment, lowest benefit |
| Base — Must + Should | 216.8 | 61.940.000 | The client figure; base cash flow for NPV |
| Optimistic — full scope | 293.2 | 84.360.000 | If the Could pool is never consumed |

The three MoSCoW tiers are the three scenarios; no invented plus-or-minus 20%.

## 2. Assumptions

| Assumption | Value | Source |
|---|---|---|
| Junior rate | 200.000 VND per hour | ITviec 2025-26 salary report |
| Senior rate | 300.000 VND per hour | ITviec 2025-26 salary report |
| Expert rate | 400.000 VND per hour | ITviec 2025-26 salary report |
| AI leverage — FE / BE / INT | 0.35 / 0.7 / 1.0 | Class parameter, recalibrated each cohort |
| Verification factor — precise / vague | 0.15 / 0.4 | Class parameter |
| Hours per function point | 3 | Class parameter — used only for the cross-check |
| Team capacity (person-hours) | 300 person-hours | 5 people x 12 weeks x 5 h |
| Money contingency on the Must set | 10% = 4.852.000 VND | Covers estimation error only. Do NOT add 20% on top of the Could pool — same buffer twice. |

## 3. tasks.md seed (effective priority Must)

Risk order: 1 = INT (external services), 2 = BE, 3 = FE. The riskiest rows start first.

| L3 ID | Layer | Person-hours | Risk order |
|---|---|---|---|
| FR-01-02-INT | INT | 4.6 | 1 |
| FR-01-03-INT | INT | 9.2 | 1 |
| FR-01-10-INT | INT | 11.2 | 1 |
| FR-01-12-INT | INT | 9.2 | 1 |
| FR-02-02-INT1 | INT | 18.4 | 1 |
| FR-02-02-INT2 | INT | 18.4 | 1 |
| FR-01-01-BE | BE | 6.8 | 2 |
| FR-01-04-BE | BE | 3.4 | 2 |
| FR-01-13-BE | BE | 3.4 | 2 |
| FR-02-01-BE | BE | 3.4 | 2 |
| FR-02-02-BE2 | BE | 8.8 | 2 |
| FR-02-05-BE | BE | 6.8 | 2 |
| FR-02-03-BE | BE | 17.6 | 2 |
| FR-02-06-BE | BE | 13.6 | 2 |
| FR-02-09-BE | BE | 3.4 | 2 |
| FR-03-03-BE | BE | 6.8 | 2 |
| FR-03-04-BE | BE | 3.4 | 2 |
| FR-01-01-FE | FE | 2.0 | 3 |
| FR-01-02-FE | FE | 2.0 | 3 |
| FR-01-03-FE | FE | 2.0 | 3 |
| FR-01-04-FE | FE | 2.0 | 3 |
| FR-01-09-FE | FE | 2.0 | 3 |
| FR-01-13-FE | FE | 2.0 | 3 |
| FR-02-09-FE | FE | 2.0 | 3 |
| FR-03-01-FE | FE | 4.0 | 3 |
| FR-03-04-FE | FE | 2.0 | 3 |

## 4. Open questions for the client

| # | Question | Blocking? | Owning feature | Default applied |
|---|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: which totals must the weekly summary contain?] | No | F-03 | Assuming sessions per workstation and hours per workstation |
| 2 | [NEEDS CLARIFICATION: is card-swipe data pulled as a daily batch or in real time?] | Yes | F-02 | Assuming a daily batch |
