```mermaid
usecase-beta
    direction LR


    actor CarOwner("Car Owner")
    actor InspectionPortal("Vehicle Inspection Portal")
    actor TrafficFinePortal("Traffic Fine Lookup Portal")
    actor SecuredRegistry("Secured Transaction Registry Portal")
    actor CarplaStaff("Carpla Staff")


    systemBoundary SellRequestQuote["Verified Vehicle Report System - Sell Request & Preliminary Quote"]
        UC01("UC-3.01: Submit Sale Request")
        UC02("UC-3.02: Re-check Legal Status")
        UC03("UC-3.03: Check Inspection Status")
        UC04("UC-3.04: Check Unpaid Traffic Fine Status")
        UC05("UC-3.05: Check Mortgage Status")
        UC06("UC-3.06: Package & Forward Verified Report and Market Price to Carpla Staff")
        UC07("UC-3.07: Review Verified Report")
        UC08("UC-3.08: Send Preliminary Quote to Car Owner")
        UC09("UC-3.09: View Preliminary Quote")
    end


    CarOwner -- UC01
    CarOwner -- UC09


    UC03 -- InspectionPortal
    UC04 -- TrafficFinePortal
    UC05 -- SecuredRegistry
    UC07 -- CarplaStaff
    UC08 -- CarplaStaff
```


    UC01 ..> : include UC02
    UC01 ..> : include UC06
