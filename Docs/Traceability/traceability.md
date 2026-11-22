# Traceability: Context → Use Case → Sequence Diagram

| Sequence Diagram<br>(Message flow) | CTX: flow-ID | USE CASE DIAGRAM: UC-ID | SEQUENCE DIAGRAM: SEQ: S-X.Y | Note |
|---|---|---|---|---|
| 01 - Vehicle Report | flow-02 | UC-1.01 | S-1.01 → S-1.04 |  |
| 01 - Vehicle Report | flow-12, flow-05 | UC-1.02 | S-1.05 → S-1.08 |  |
| 01 - Vehicle Report | flow-12, flow-05 | UC-1.03 | S-1.09 → S-1.12 |  |
| 02 - Maintenance & Repair History and Upload External Invoice | flow-13, flow-05 | UC-1.04 | S-2.01 → S-2.04 |  |
| 02 - Maintenance & Repair History and Upload External Invoice | flow-03, flow-17, flow-18, flow-05 | UC-1.05 | S-2.05 → S-2.11 |  |
| 03 - View Battery State of Health | flow-14, flow-05 | UC-1.06 | S-3.01 → S-3.05 |  |
| 04 - Refresh Legal Status | flow-04, flow-05 | UC-1.07 | S-4.01, S-4.08, S-4.09 |  |
| 04 - Refresh Legal Status | flow-23, flow-24 | UC-1.08 | S-4.02 → S-4.03 |  |
| 04 - Refresh Legal Status | flow-19, flow-20 | UC-1.09 | S-4.04 → S-4.05 |  |
| 04 - Refresh Legal Status | flow-21, flow-22 | UC-1.10 | S-4.06 → S-4.07 |  |
| 05 - View Market Price and 12 Week Chart | flow-08 | UC-2.01 | S-5.01 → S-5.15 |  |
| 05 - View Market Price and 12 Week Chart | flow-08 | UC-2.02 | S-5.16 → S-5.22 |  |
| 06 - Calculate Weekly Market Price | - | UC-2.04 | S-6.01 |  |
| 06 - Calculate Weekly Market Price | flow-15, flow-16 | UC-2.05 | S-6.02 → S-6.10 |  |
| 06 - Calculate Weekly Market Price | - | UC-2.07 | S-6.11 → S-6.15 |  |
| 06 - Calculate Weekly Market Price | flow-12 | UC-2.03 | S-6.16 → S-6.19 |  |
| 06 - Calculate Weekly Market Price | - | UC-2.08 | S-6.20 → S-6.24 |  |
| 06 - Calculate Weekly Market Price | - | UC-2.09 | S-6.25 → S-6.27 |  |
| 07 - 12-week Backfill and 180-day Sync | flow-08 | UC-2.10 | S-7.01 → S-7.03, S-7.20, S-7.21 |  |
| 07 - 12-week Backfill and 180-day Sync | flow-15, flow-16 | UC-2.06 | S-7.04 → S-7.10 |  |
| 07 - 12-week Backfill and 180-day Sync | flow-12 | UC-2.03 | S-7.11 → S-7.13 |  |
| 07 - 12-week Backfill and 180-day Sync | - | UC-2.07 | S-7.14 → S-7.15 |  |
| 07 - 12-week Backfill and 180-day Sync | - | UC-2.08 | S-7.16 → S-7.17 |  |
| 07 - 12-week Backfill and 180-day Sync | - | UC-2.09 | S-7.18 → S-7.19 |  |
| 08 - Stop Tracking | - | UC-2.11 | S-8.01 → S-8.05 |  |
| 09 - Submit Sale Request | flow-01 | UC-3.01 | S-9.01, S-9.02, S-9.13 |  |
| 09 - Submit Sale Request | - | UC-3.02 | S-9.09, S-9.10 |  |
| 09 - Submit Sale Request | flow-23, flow-24 | UC-3.03 | S-9.03 → S-9.04 |  |
| 09 - Submit Sale Request | flow-19, flow-20 | UC-3.04 | S-9.05 → S-9.06 |  |
| 09 - Submit Sale Request | flow-21, flow-22 | UC-3.05 | S-9.07 → S-9.08 |  |
| 09 - Submit Sale Request | flow-09, flow-10 | UC-3.06 | S-9.11 → S-9.12 |  |
| 10 - Staff Review and Car Owner Preliminary Quote | flow-09, flow-10 | UC-3.07 | S-10.01 → S-10.03 |  |
| 10 - Staff Review and Car Owner Preliminary Quote | flow-11 | UC-3.08 | S-10.04 → S-10.05 |  |
| 10 - Staff Review and Car Owner Preliminary Quote | flow-07 | UC-3.09 | S-10.06 → S-10.09 |  |
