  
sequenceDiagram  
    autonumber  
    actor CO as Car Owner  
    participant SYS as TASCO Mobility System  
    participant VIP as Vehicle Inspection Portal  
    participant TFP as Traffic Fine Lookup Portal  
    participant STR as Secured Transaction Registry Portal  
    participant CRM as Carpla CRM

    Note over CO,SYS: UC-3.01 \- Submit Sale Request  
    CO-\>\>SYS: Enter  desired price, current ODO and phone number  
    CO-\>\>SYS: Click Submit sale request

    Note over SYS,STR: UC-3.02 \- Re-check Legal Status (real-time)  
    par UC-3.03 \- Check Inspection Status  
        SYS-\>\>VIP: Query plate/VIN  
        VIP--\>\>SYS: Inspection validity  
    and UC-3.04 \- Check Unpaid Traffic Fine Status  
        SYS-\>\>TFP: Query unpaid fines  
        TFP--\>\>SYS: Fine status  
    and UC-3.05 \- Check Mortgage Status  
        SYS-\>\>STR: Query mortgage status  
        STR--\>\>SYS: Mortgage status  
    end

    alt Any API gateway fails  
        SYS--\>\>CO: Block submit and show error  
    else All checks succeed  
        opt Vehicle is mortgaged  
            SYS-\>\>SYS: Attach Red Flag to report  
        end  
        Note over SYS,CRM: UC-3.06 \- Package & Forward Verified Report and Market Price to Carpla Staff  
        SYS-\>\>SYS: Bundle desired price, current ODO, phone number and verified vehicle report (identification & status)  
        SYS-\>\>CRM: Forward data package  
        SYS--\>\>CO: Request submitted successfully  
    end

