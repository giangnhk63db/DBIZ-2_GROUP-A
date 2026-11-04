  
sequenceDiagram  
    autonumber  
    actor CO as Car Owner  
    participant SYS as TASCO Mobility System  
    participant VETC as VETC

    Note over CO,SYS: UC-1.01 \- Enter VIN to  initialize Report  
    CO-\>\>SYS: Select Create Verified Vehicle Report  
    CO-\>\>SYS: Enter 17-character VIN  
    SYS-\>\>SYS: Initialize digital vehicle profile  
    SYS--\>\>CO: Vehicle profile created

    Note over CO,VETC: UC-1.02 \- View Vehicle Identification  
    CO-\>\>SYS: Open Vehicle Identification  
    SYS-\>\>VETC: Query core vehicle identity data  
    VETC--\>\>SYS: Return identity data  
    SYS--\>\>CO: Display verified identification

    Note over CO,VETC: UC-1.03 \- View Verified Minimum Mileage  
    CO-\>\>SYS: Request vehicle mileage  
    SYS-\>\>VETC: Retrieve logged mileage records  
    VETC--\>\>SYS: Return mileage records  
    SYS--\>\>CO: Display verified minimum ODO

