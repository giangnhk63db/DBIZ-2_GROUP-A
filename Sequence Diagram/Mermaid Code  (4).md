### 

sequenceDiagram

    autonumber

    actor Owner as Car Owner

    participant App as Verified Vehicle Report

    participant MP as Market Price Service

    participant DB as Weekly Price DB

    Note over Owner, DB: UC-2.01 \- View Market Price

    Owner-\>\>App: Open the Market Price section

    App-\>\>MP: Request the vehicle's market price

    MP-\>\>DB: Retrieve the latest weekly price record

    DB--\>\>MP: Return the weekly price record

    alt Status OK

        MP--\>\>App: Median price, P25-P75, trend, confidence level, listing count, update date, minimum verified mileage

        App--\>\>Owner: Display the price, the label "Asking Prices for Similar Vehicles", and a trend arrow with the percentage change

        App--\>\>Owner: Display a note that actual mileage may be higher, which could result in a lower price

        App--\>\>Owner: Display a disclaimer that the reference price is based on asking prices and is not Carpla's purchase price

        opt Electric Vehicle

            App--\>\>Owner: Display a note that the price has not been adjusted for battery State of Health (SoH)

        end

    else INSUFFICIENT\_DATA

        MP--\>\>App: Return status INSUFFICIENT\_DATA

        App--\>\>Owner: Display "Insufficient data to calculate the market price" (or a notice indicating insufficient verified mileage data)

    else New Vehicle Model with No Existing Listings

        MP--\>\>App: Return status "Calculating"

        App--\>\>Owner: Display "Calculating Price"

    else Latest Synchronization Failed Across All Marketplaces

        MP--\>\>App: Return the previous price record

        App--\>\>Owner: Display the previous record with the label "Last Updated: dd/mm"

    end

    Note over Owner, DB: UC-2.02 \- View 12-Week Price Trend Chart

    App-\>\>MP: Request median prices for the latest 12 weeks

    MP-\>\>DB: Query 12 weekly records

    DB--\>\>MP: Return the list of records

    alt Sufficient Historical Data Available

        MP--\>\>App: Return 12-week data

        App--\>\>Owner: Render the chart, leave INSUFFICIENT\_DATA weeks blank, and label back-calculated data as "Estimated"

        App--\>\>Owner: Display a chart note explaining market fluctuations and the impact of additional mileage driven

    else Insufficient Historical Data

        App--\>\>Owner: Display "No Historical Data Available"

    end