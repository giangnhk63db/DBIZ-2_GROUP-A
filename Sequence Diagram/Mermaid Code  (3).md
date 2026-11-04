  
sequenceDiagram  
    autonumber  
    actor CO as Car Owner  
    participant SYS as TASCO Mobility System  
    participant VIP as Vehicle Inspection Portal  
    participant TFP as Traffic Fine Lookup Portal  
    participant STR as Secured Transaction Registry Portal

    Note over CO,STR: UC-1.07 \- Refresh Legal Status  
    CO-\>\>SYS: Click Refresh Legal Status

    par UC-1.08 \- View Inspection Status  
        SYS-\>\>VIP: Send plate and VIN  
        VIP--\>\>SYS: Registration and inspection validity  
    and UC-1.09 \- View Unpaid Traffic Fine Status  
        SYS-\>\>TFP: Send plate  
        TFP--\>\>SYS: Outstanding unpaid fines  
    and UC-1.10 \- View Mortgage Status  
        SYS-\>\>STR: Send VIN  
        STR--\>\>SYS: Mortgage status  
    end

    SYS-\>\>SYS: Update Verified Vehicle Report  
    SYS--\>\>CO: Display latest legal status

