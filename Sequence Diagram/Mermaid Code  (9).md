  
sequenceDiagram  
    autonumber  
    participant CRM as Carpla CRM  
    actor CS as Carpla Staff  
    participant SYS as TASCO Mobility System  
    actor CO as Car Owner

    Note over CRM,CS: UC-3.07 \- Review Verified Report  
    CS-\>\>CRM: Open internal system  
    CRM--\>\>CS: Show Verified Report **(**Identification & Legal status**)**, desired price, market price, current ODO and phone number  
    CS-\>\>CS: Evaluate maintenance, fines, mortgage, price

    Note over CS,SYS: UC-3.08 \- Send Preliminary Quote to Car Owner  
    CS-\>\>CRM: Input suggested quote range  
    CRM-\>\>SYS: Submit preliminary quote

    Note over SYS,CO: UC-3.09 \- View Preliminary Quote  
    SYS--\>\>CO: Push notification  
    CO-\>\>SYS: Open app and view quote  
    SYS--\>\>CO: Display preliminary quote  
    CO-\>\>CO: Decide to accept and book Physical Inspection at showroom  
