# Use Case Diagram: Overview

| Field | Value |
|---|---|
| Version | v2 |
| Actors | Car owner |

```mermaid
usecase-beta
    direction LR

    actor Owner("Car owner")

    systemBoundary Overview["Verified Vehicle Report - Overview"]
        UC01("UC-0.01: Create Vehicle Report")
        UC02("UC-0.02: View Vehicle Identification & Status")
        UC03("UC-0.03: View Market Price")
        UC04("UC-0.04: Request Sale & Preliminary Quote")
    end

    Owner -- UC01
    Owner -- UC02
    Owner -- UC03
    Owner -- UC04

    UC02 ..> : include UC01
    UC03 ..> : include UC01
    UC04 ..> : include UC01
```
The full use case inventory, with features and login rules, is W0 Table 1 in
[w0-scope.md](../Functionlist/w0-scope.md).
