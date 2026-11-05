```mermaid
usecase-beta
    direction LR

    actor Owner("Car owner")
    actor Timer("Weekly timer")
    actor Nextgen("Nextgen VN")
    actor ChoTot("Chợ Tốt")
    actor VETC("VETC")

    systemBoundary MarketPrice["Verified Vehicle Report - Market Price"]
        UC11("UC-2.11: Stop Market Price Tracking")
        UC01("UC-2.01: View Market Price")
        UC02("UC-2.02: View 12-Week Price Trend Chart")
        UC04("UC-2.04: Calculate Weekly Market Price")
        UC05("UC-2.05: Sync Listings from Partner Marketplaces")
        UC06("UC-2.06: Sync 180-Day Listing History (for the 1st time signed up car)")
        UC07("UC-2.07: Normalize and Clean Listings")
        UC03("UC-2.03: Check Verified Mileage Plausibility")
        UC08("UC-2.08: Select Comparable Vehicles")
        UC10("UC-2.10: Backfill 12-Week Price History")
        UC09("UC-2.09: Calculate Market Price and Confidence")
    end

    Owner -- UC11
    Owner -- UC01
    Owner -- UC10
    Timer -- UC04

    UC05 -- Nextgen
    UC06 -- Nextgen
    UC05 -- ChoTot
    UC06 -- ChoTot
    UC03 -- VETC
    UC10 -- VETC

    UC01 ..> : include UC02
   
    UC04 ..> : include UC05
    UC04 ..> : include UC07
    UC04 ..> : include UC03
    UC04 ..> : include UC08
    UC04 ..> : include UC09

    UC10 ..> : include UC07
    UC10 ..> : include UC08
    UC10 ..> : include UC09

    UC06 ..> : extend UC10


```

