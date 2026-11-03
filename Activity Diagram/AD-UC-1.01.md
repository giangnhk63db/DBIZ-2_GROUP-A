**Activity Diagram: AD-UC-1.01 Enter VIN to initialize Report**

*1\. Diagram*
```mermaid
swimlane-beta TB

subgraph CAR\["Car Owner"\]

start@{ shape: sm-circ }

a1**(**"Select Create Verified Vehicle Report"**)**

a2**(**"Enter 17-character VIN"**)**

a3**(**"View validation error"**)**

a4**(**"Receive profile created confirmation"**)**

stop1@{ shape: fr-circ }

stop2@{ shape: fr-circ }

end

subgraph SYS\["TASCO Mobility System"\]

s1**(**"Client-side format validation (FR-01-01-FE**)**")

d1{"Is VIN format valid?"}

s2**(**"Display format error feedback (FR-01-01-FE**)**")

s3**(**"Validate checksum logic & check existence (FR-01-01-BE**)**")

d2{"Does VIN exist?"}

s4**(**"Display error 'Số VIN không tồn tại' (FR-01-01-FE**)**")

s5**(**"Return vehicle profile created state (FR-01-01-FE**)**")

end

start \--\> a1 \--\> a2 \--\> s1 \--\> d1

d1 \--\>\|"no"\| s2 \--\> a3 \--\> stop1

d1 \--\>\|"yes"\| s3 \--\> d2

d2 \--\>\|"no"\| s4 \--\> a3

d2 \--\>\|"yes"\| s5 \--\> a4 \--\> stop2
```
**Reading notes:**

- The workflow strictky prioritizes client-side format validation (FR-01-01-FE) as the primary gatekeeper to filter out invalid inputs before initiating any backend database interactions.  
- A secondary backend validation (FR-01-01-BE) verifies the checksum logic and actual database existence. If the VIN does not exist, the system actively triggers a specific error message "Số VIN không tồn tại" (FR-01-01-FE).  
- Both validation failure paths (format error and non-existent VIN) are optimized to converge into a single user action (View validation error) before gracefully terminating the flow.  
- Order inside the flow mirrors Sequence Diagram interactions (SEQ S-1.01 to S-1.04), strictly isolating this initialization phase from external VETC queries.

**2\. Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Select Create Verified Vehicle Report | SEQ S-1.01 |
| a2 | Action | Enter 17-character VIN | FR-01-01-FE / SEQ S-1.02 |
| s1 | Action | Client-side format validation | FR-01-01-FE |
| d1 | Decision | Is VIN format valid? | FR-01-01-FE |
| s2 | Action | Display format error feedback | FR-01-01-FE |
| s3 | Action | Validate checksum logic & check existence | FR-01-01-BE / SEQ S-1.03 |
| d2 | Decision | Does VIN exist? | FR-01-01-BE |
| s4 | Action | Display error 'Số VIN không tồn tại' | FR-01-01-FE |
| s5 | Action | Return vehicle profile created state | FR-01-01-FE / SEQ S-1.04 |
| a3 | Action | View validation error | N/A |
| a4 | Action | Receive profile created confirmation | N/A |

