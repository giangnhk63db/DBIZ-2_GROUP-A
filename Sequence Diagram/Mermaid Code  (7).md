### 

sequenceDiagram  
    autonumber  
    actor Owner as Car Owner  
    participant App as Verified Vehicle Report  
    participant MP as Market Price Service  
    participant PDB as Weekly Price DB

    Note over Owner,PDB: UC-2.11 \- Stop Market Price Tracking  
    Owner-\>\>App: Delete the vehicle or mark it as sold  
    App-\>\>MP: Request to stop price tracking  
    MP-\>\>MP: Remove the vehicle from the weekly job  
    MP--\>\>App: Confirm request  
    App--\>\>Owner: Market price tracking has been stopped  
