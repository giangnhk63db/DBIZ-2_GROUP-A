```mermaid
usecase-beta
    direction LR

    actor CarOwner("Car Owner")
    actor VETC("VETC")
    actor CarplaService("Carpla Service")
    actor EInvoicePortal("Electronic Invoice Portal")
    actor EVCharging("Electric Vehicle Charging Stations")
    actor InspectionPortal("Vehicle Inspection Portal")
    actor TrafficFinePortal("Traffic Fine Lookup Portal")
    actor SecuredRegistry("Secured Transaction Registry Portal")

    systemBoundary VehicleIDStatus["Verified Vehicle Report System - Vehicle Identification & Status"]
        UC01("UC-1.01: Enter VIN to initialize Report")
        UC02("UC-1.02: View Vehicle Identification")
        UC03("UC-1.03: View Verified Minimum Mileage")
        UC04("UC-1.04: View Maintenance & Repair History in Carpla Service")
        UC05("UC-1.05: Upload External Maintenance & Repair Invoices")
        UC06("UC-1.06: View Battery State of Health (SoH)")
        UC07("UC-1.07: Refresh Legal Status")
        UC08("UC-1.08: View Inspection Status")
        UC09("UC-1.09: View Unpaid Traffic Fine Status")
        UC10("UC-1.10: View Mortgage Status")
    end

    CarOwner -- UC01
    CarOwner -- UC02
    CarOwner -- UC03
    CarOwner -- UC04
    CarOwner -- UC05
    CarOwner -- UC06
    CarOwner -- UC07
    CarOwner -- UC08
    CarOwner -- UC09
    CarOwner -- UC10

    UC02 -- VETC
    UC03 -- VETC
    UC04 -- CarplaService
    UC05 -- EInvoicePortal
    UC06 -- EVCharging
    UC07 -- InspectionPortal
    UC07 -- TrafficFinePortal
    UC07 -- SecuredRegistry

    UC05 ..> : extend UC04
    UC08 ..> : include UC07
    UC09 ..> : include UC07
    UC10 ..> : include UC07
```
