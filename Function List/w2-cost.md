# W2 Costing and roll-up: 
Fully derived from the W1 function list (`w1-function-list.md`), the effective priorities in the W4_Prioritization sheet (no priority changes are recorded there yet, so they equal the initial priorities) and the class Parameters:

- Person-hours = base hours x AI leverage factor + base hours x verification factor.
- AI leverage: FE 0.35, BE 0.7, INT 1.0. Verification factor: 0.15 when the EARS criterion is precise, 0.4 when it is vague.
- Rate: FE Junior 200.000 VND/h; BE and INT Senior 300.000 VND/h.

## Cost per L3 work item

| L3 ID | Feature | Layer | Base h | AI-adj. h | Verif. h | Person-hours | Band | Cost (VND) | Effective priority |
|---|---|---|---|---|---|---|---|---|---|
| FR-01-01-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-01-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-01-02-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-02-INT | F-01 | INT | 4 | 4.0 | 0.6 | 4.6 | Senior | 1.380.000 | Must |
| FR-01-03-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-03-INT | F-01 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Must |
| FR-01-04-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-04-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-05-FE | F-01 | FE | 4 | 1.4 | 1.6 | 3.0 | Junior | 600.000 | Should |
| FR-01-05-BE | F-01 | BE | 4 | 2.8 | 1.6 | 4.4 | Senior | 1.320.000 | Should |
| FR-01-06-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Could |
| FR-01-07-INT | F-01 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Could |
| FR-01-07-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Could |
| FR-01-08-FE | F-01 | FE | 4 | 1.4 | 1.6 | 3.0 | Junior | 600.000 | Could |
| FR-01-08-INT | F-01 | INT | 8 | 8.0 | 3.2 | 11.2 | Senior | 3.360.000 | Could |
| FR-01-08-BE | F-01 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Could |
| FR-01-09-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-10-INT | F-01 | INT | 8 | 8.0 | 3.2 | 11.2 | Senior | 3.360.000 | Must |
| FR-01-11-INT | F-01 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Should |
| FR-01-12-INT | F-01 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Must |
| FR-01-13-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-13-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-01-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-02-INT1 | F-02 | INT | 16 | 16.0 | 2.4 | 18.4 | Senior | 5.520.000 | Must |
| FR-02-02-INT2 | F-02 | INT | 16 | 16.0 | 2.4 | 18.4 | Senior | 5.520.000 | Must |
| FR-02-02-BE2 | F-02 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Must |
| FR-02-05-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-02-03-BE | F-02 | BE | 16 | 11.2 | 6.4 | 17.6 | Senior | 5.280.000 | Must |
| FR-02-04-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Should |
| FR-02-06-BE | F-02 | BE | 16 | 11.2 | 2.4 | 13.6 | Senior | 4.080.000 | Must |
| FR-02-07-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Could |
| FR-02-07-INT | F-02 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Could |
| FR-02-08-INT | F-02 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Could |
| FR-02-08-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Could |
| FR-02-09-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-09-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-10-FE | F-02 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Should |
| FR-02-10-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Should |
| FR-02-11-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Could |
| FR-02-11-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Could |
| FR-03-01-FE | F-03 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-03-02-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Should |
| FR-03-03-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-03-04-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-04-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-05-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Should |
| FR-03-05-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Should |
| FR-03-06-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Should |
| FR-03-06-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Should |

## Roll-up per feature

| Feature | Name | L2 | L3 | Base h | Must p-h | Should p-h | Could p-h | In-scope p-h | In-scope cost (VND) | Must share | Transactional FP | Base h per FP |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| F-01 | Vehicle Passport | 13 | 22 | 124 | 59.8 | 16.6 | 42.4 | 118.8 | 33.840.000 | 50% | 51 | 2.4 |
| F-02 | Market Price | 12 | 18 | 144 | 92.4 | 10.8 | 34.0 | 137.2 | 40.360.000 | 67% | 37 | 3.9 |
| F-03 | Sell Request & Preliminary Quote | 6 | 9 | 52 | 16.2 | 21.0 | 0.0 | 37.2 | 10.160.000 | 44% | 21 | 2.5 |
| **Project** | | **31** | **49** | **320** | **168.4** | **48.4** | **76.4** | **293.2** | **84.360.000** | **57%** | **109** | **2.9** |

## The three headline figures

| Scenario | Scope | Person-hours | Cost (VND) | Meaning |
|---|---|---|---|---|
| Committed | Must only | 168.4 | 48.520.000 | What the team guarantees to deliver — the Minimum Usable SubseT |
| Expected | Must + Should | 216.8 | 61.940.000 | The figure given to the client; the base cash flow for Session 8 |
| Ceiling | Must + Should + Could | 293.2 | 84.360.000 | If the Could contingency pool is never consumed |

## Threshold check (after W4)

| Indicator | Value | Threshold | Verdict |
|---|---|---|---|
| Must share of in-scope effort | 57.4% | at most 60% | PASS |
| Could share (contingency pool) | 26.1% | target 20% | PASS |
| In-scope effort vs team capacity | 293.2 p-h | at most 300 p-h | PASS |
| Hidden technical work, share of effort | 15.1% | at least 10% | PASS |
| L3 rows above 16 hours | 0 | 0 | PASS |
| Feature count | 3 | 3 to 6 | PASS |
| Function-point cross-check: 212 FP (109 transactional + 103 data) x 3 h = 636 h, plus 56 h non-functional, = 692 h vs 320 bottom-up base h | 53.8% apart | at most 30% | REVIEW |

Three observations the team carries into W4:

_Not filled in yet in the Excel file._
