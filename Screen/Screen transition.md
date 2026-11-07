**Screen transition**

1. **Car owner**  
   \---  
   config:  
    layout: elk  
    elk:  
      nodePlacementStrategy: NETWORK\_SIMPLEX  
      mergeEdges: false  
    flowchart:  
      nodeSpacing: 110  
      rankSpacing: 240  
      padding: 40  
      curve: linear  
   \---  
   flowchart LR  
    entry(\["Entry: Car owner signed in\<br\>to Tasco Mobility"\])  
    entry\_quote(\["Entry: Carpla staff\<br\>sent quote"\])  
     
    S00\_01\["S-F00-01\<br\>Home"\]  
    S01\_01\["S-F01-01\<br\>Create Vehicle Report\<br\>(Enter VIN)"\]  
    S01\_02\["S-F01-02\<br\>Vehicle Profile"\]  
    S01\_03\["S-F01-03\<br\>Verified Minimum Mileage"\]  
    S01\_05\["S-F01-05\<br\>Maintenance History"\]  
    S01\_06\["S-F01-06\<br\>Battery Health (SoH)"\]  
    S02\_01\["S-F02-01\<br\>Market Price &\<br\>12\-Week Trend"\]  
    S03\_01\["S-F03-01\<br\>Submit Sale Request"\]  
    S03\_03\["S-F03-03\<br\>Sale Request &\<br\>Preliminary Quote"\]  
     
    entry \--\> S00\_01  
     
    S00\_01 \--\>\|"clicks Create Verified\<br\>Vehicle Report"\| S01\_01  
    S00\_01 \--\>\|"selects an\<br\>existing vehicle"\| S01\_02  
    S01\_01 \--\>\|"\[VIN accepted,\<br\>profile created\]"\| S01\_02  
    S01\_01 \-.\-\>\|"back"\| S00\_01  
    S01\_02 \-.\-\>\|"back"\| S00\_01  
     
    S01\_02 \--\>\|"clicks View details\<br\>(Minimum mileage)"\| S01\_03  
    S01\_03 \-.\-\>\|"back"\| S01\_02  
     
    S01\_02 \--\>\|"clicks View all\<br\>(Maintenance)"\| S01\_05  
    S01\_05 \--\>\|"uploads invoice\<br\>\[timeline updated\]"\| S01\_05  
    S01\_05 \-.\-\>\|"back"\| S01\_02  
     
    S01\_02 \--\>\|"clicks View all\<br\>(Battery health)"\| S01\_06  
    S01\_06 \-.\-\>\|"back"\| S01\_02  
     
    S01\_02 \--\>\|"clicks Refresh\<br\>\[legal status updated\]"\| S01\_02  
     
    S01\_02 \--\>\|"clicks View trend"\| S02\_01  
    S02\_01 \-.\-\>\|"back"\| S01\_02  
     
    S01\_02 \--\>\|"clicks Submit\<br\>sale request"\| S03\_01  
    S03\_01 \-.\-\>\|"back"\| S01\_02  
    S03\_01 \--\>\|"clicks Submit\<br\>\[sale request saved\]"\| S03\_03  
     
    entry\_quote \--\>\|"\[quote sent\]"\| S03\_03  
    S03\_03 \-.\-\>\|"back\<br\>(after Submit)"\| S01\_02  
    S03\_03 \-.\-\>\|"back\<br\>(from notification)"\| S00\_01  
     
    classDef screen fill:\#f2c9c9,stroke:\#8b1a10,stroke-width:2px,color:\#000;  
    classDef entryNode fill:\#fdf0b8,stroke:\#c9a400,stroke-width:2px,color:\#000;  
    class S00\_01,S01\_01,S01\_02,S01\_03,S01\_05,S01\_06,S02\_01,S03\_01,S03\_03 screen;  
    class entry,entry\_quote entryNode;  
     
2. **Carpla staff**  
     
   \---  
   config:  
    layout: elk  
    flowchart:  
      nodeSpacing: 110  
      rankSpacing: 200  
      padding: 40  
   \---  
   flowchart LR  
    entry\_staff(\["Entry: Receives a new\<br\>vehicle sale request"\])  
    S03\_02\["S-F03-02\<br\>Sale Request Review & Quote"\]  
    exit\_staff(\["Exit: Quote sent\<br\>to Car Owner"\])  
     
    entry\_staff \--\>\|"Opens the sale request"\| S03\_02  
    S03\_02 \--\>\|"Enters suggested quote range\<br\>and sends it"\| exit\_staff  
    S03\_02 \-.\-\>\|"back"\| exit\_staff  
     
    classDef screen fill:\#f2c9c9,stroke:\#8b1a10,stroke-width:2px,color:\#000;  
    classDef entryNode fill:\#fdf0b8,stroke:\#c9a400,stroke-width:2px,color:\#000;  
    class S03\_02 screen;  
    class entry\_staff,exit\_staff entryNode;  
   