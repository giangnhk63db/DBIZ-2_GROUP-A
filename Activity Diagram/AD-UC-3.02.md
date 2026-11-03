# **Activity Diagram: AD-UC-3.03**

1. ## **Code**

swimlane-beta TB

 subgraph CO\["Car Owner"\]

start@{ shape: sm-circ }

a1**(**"Submit sale request (FR-03-01-FE**)**")

end

subgraph TMS\["Tasco Mobility System"\]

s1**(**"Query Inspection status (FR-03-02-BE**)**")

s2**(**"Return Inspection status (FR-03-02-BE**)**")

s3**(**"Insert \#quot;No data\#quot; in report (FR-03-02-BE**)**")

s4**(**"Block submit sale request and show error (FR-03-02-BE**)**")

stop1@{ shape: fr-circ }

stop2@{ shape: fr-circ }

stop3@{ shape: fr-circ }

end

subgraph VIP\["Vehicle Inspection Portal"\]

v1**(**"Look up inspection status (FR-03-02-INT**)**")

d1{" "}

end

start \--\> a1 \--\> s1 \--\> v1 \--\> d1

d1 \--\>\|"Have data"\| s2 \--\> stop1

d1 \--\>\|"No Data"\| s3 \--\> stop2

d1 \--\>\|"API fail"\| s4 \--\> stop3

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Submit sale request | FR-03-01-FE |
| s1 | Action | Query Inspection status | FR-03-02-BE |
| v1 | Action | Look up inspection status | FR-03-02-INT |
| d1 | Decision | Guards “Have data” / “No Data” / “API fail” | (none) |
| s2 | Action | Return Inspection status | FR-03-02-BE |
| s3 | Action | Insert “No data” in report | FR-03-02-BE |
| s4 | Action | Block submit sale request and show error | FR-03-02-BE |

