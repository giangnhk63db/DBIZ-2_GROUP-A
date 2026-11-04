### 

sequenceDiagram

    autonumber

    participant Timer as Weekly Timer

    participant MP as Market Price Service

    participant VETC as VETC

    participant NG as Nextgen VN

    participant CT as Cho Tot

    participant LDB as Listing Store

    participant PDB as Weekly Price DB

   

    Note over Timer,PDB: UC-2.04 \- Calculate Weekly Market Price

    Timer-\>\>MP: Trigger weekly job at a fixed time

    loop Each vehicle model **(**make**,** model**)**

        Note over MP,PDB: UC-2.05 \- Sync Listings from Partner Marketplaces

        par Synchronize Nextgen VN

            MP-\>\>NG: Request incremental sync **(**make and model only**)**

            alt Sync failed

                loop Up to 3 retries, 6 hours apart

                    MP-\>\>NG: Retry synchronization

                end

            end

            NG--\>\>MP: Return new, price-updated, and removed listings

        and Synchronize Cho Tot

            MP-\>\>CT: Request incremental sync **(**make and model only**)**

            alt Sync failed

                loop Up to 3 retries, 6 hours apart

                    MP-\>\>CT: Retry synchronization

                end

            end

            CT--\>\>MP: Return new, price-updated, and removed listings

        end

        MP-\>\>LDB: Store listings with source marketplace, original listing ID, posting date, removal date, and anonymized seller ID

        MP-\>\>MP: Determine valid data sources for the current week

        alt All marketplace synchronizations failed

            MP-\>\>PDB: Record status API\_FAILED

        else At least one marketplace synchronization succeeded

            Note over MP,LDB: UC-2.07 \- Normalize and Clean Listings

            MP-\>\>LDB: Retrieve listings for the vehicle model

            MP-\>\>MP: Map make and model to the TASCO catalog, normalize prices to VND and mileage to km

            MP-\>\>MP: Retain listings active within the last 90 days

            MP-\>\>MP: Exclude listings with missing fields, zero prices, or negotiable prices

            MP-\>\>MP: Remove cross-marketplace duplicates and retain the most recent listing

            loop Each vehicle in the model group

                Note over MP,VETC: UC-2.03 \- Check Verified Mileage Plausibility

                MP-\>\>VETC: Retrieve minimum verified mileage and manufacturing year

                VETC--\>\>MP: Return minimum verified mileage and manufacturing year

                MP-\>\>MP: Compare minimum verified mileage divided by vehicle age **(**minimum 1 year**)** against the configured threshold

                alt Mileage below threshold

                    MP-\>\>PDB: Record status INSUFFICIENT\_DATA

                else Mileage is plausible

                    Note over MP,PDB: UC-2.08 \- Select Comparable Vehicles

                    MP-\>\>MP: Select Level 0 listings **(**year ±1**,** mileage ±20%**,** minimum mileage ±10**,**000 km**)**

                    MP-\>\>MP: Remove price outliers outside Q1 \- 1.5 x IQR to Q3 \+ 1.5 x IQR

                    loop Until 30 listings are collected or Level 2 is reached

                        alt Fewer than 30 listings and expansion is available

                            MP-\>\>MP: Expand to Level 1 **(**year ±2**)**

                            MP-\>\>MP: Expand to Level 2 **(**mileage ±40%**,** minimum mileage ±20**,**000 km**)**

                        end

                    end

                    alt Fewer than 30 listings after Level 2

                        MP-\>\>PDB: Record status INSUFFICIENT\_DATA

                    else Sufficient sample size

                        Note over MP,PDB: UC-2.09 \-Calculate Market Price and Confidence

                        MP-\>\>MP: Calculate median and P25-P75, round to the nearest million VND

                        MP-\>\>MP: Assign confidence level: High, Medium, or Low

                        MP-\>\>PDB: Save weekly price record

                    end

                end

            end

        end

    end

