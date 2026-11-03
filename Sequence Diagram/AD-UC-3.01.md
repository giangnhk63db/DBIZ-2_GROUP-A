# **Activity Diagram: AD-UC-3.01**

1. ## **Code**

swimlane-beta TB

 subgraph CO\["Car Owner"\]

start@{ shape: sm-circ }

a1**(**"Submit sale request with entering mileage**,** desired price and phone number (FR-03-01-FE**)**")

end

subgraph TMS\["Tasco Mobility System"\]

s1**(**"Receive sale request for the verified report (FR-03-01-BE**)**")

d1{" "}

s2**(**"Create sale request (FR-03-01-BE**)**")

s3**(**"Notify car owner that the sale request is being processed (FR-03-01-FE**)**")

s4**(**"Reject request and return error (FR-03-01-BE**)**")

stop1@{ shape: fr-circ }

stop2@{ shape: fr-circ }

end

start \--\> a1 \--\> s1 \--\> d1

d1 \--\>\|"Report initialized for VIN"\| s2 \--\> s3 \--\> stop1

d1 \--\>\|"Report not initialized for VIN"\| s4 \--\> stop2

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Submit sale request with entering mileage, desired price and phone number | FR-03-01-FE |
| s1 | Action | Receive sale request for the verified report | FR-03-01-BE |
| d1 | Decision | Guards “Report initialized for VIN” / “Report not initialized for VIN” | (none) |
| s2 | Action | Create sale request | FR-03-01-BE |
| s3 | Action | Notify car owner that the sale request is being processed | FR-03-01- FE |
| s4 | Action | Reject request and return error | FR-03-01-BE |

