  
sequenceDiagram  
    autonumber  
    actor CO as Car Owner (EV)  
    participant SYS as TASCO Mobility System  
    participant EVC as EV Charging Stations

    Note over CO,EVC: UC-1.06 \- View Battery State of Health (SoH)  
    CO-\>\>SYS: Open battery health (SoH)  
    SYS-\>\>EVC: Retrieve charging session data  
    EVC--\>\>SYS: Return charging sessions  
    SYS-\>\>SYS: Calculate SoH (current vs original charging time)  
    SYS--\>\>CO: Display SoH percentage  
