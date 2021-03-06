# PySpark ETL Pipeline
#### The script's objective is to:
1) Push the input JSON data sets into PySpark DataFrames:

### Data Set 1 - stations:
```javascript
{
    "stations": {
        "internal_bus_station_id": [
            0, 1, 2, 3, 4, 5, 6, 7, 8, 9
        ], 
        "public_bus_station": [
            "BAutogara", "BVAutogara", "SBAutogara", "CJAutogara", "MMAutogara","ISAutogara", "CTAutogara", "TMAutogara", "BCAutogara", "MSAutogara"
        ]
    }
}
```
### Data Set 2 - trips:
```javascript
{
    "trips": {
            "origin": [
                "B","B","BV","TM","CJ"
            ],
            "destination": [
                "SB","MM", "IS","CT","SB"
            ],
            "internal_bus_station_ids": [
                [0,2],[0,2,4],[1,8,3,5],[7,2,9,4,6],[3,9,5,6,7,8]
            ],
            "triptimes": [
                ["2021-03-01 06:00:00", "2021-03-01 09:10:00"],
                ["2021-03-01 10:10:00", "2021-03-01 12:20:10", "2021-03-01 14:10:10"],
                ["2021-04-01 08:10:00", "2021-04-01 12:20:10", "2021-04-01 15:10:00", "2021-04-01 15:45:00"],
                ["2021-05-01 10:45:00", "2021-05-01 12:20:10", "2021-05-01 18:30:00", "2021-05-01 20:45:00", "2021-05-01 22:00:00"],
                ["2021-05-01 07:10:00", "2021-05-01 10:20:00", "2021-05-01 12:30:00", "2021-05-01 13:25:00", "2021-05-01 14:35:00", "2021-05-01 15:45:00"]
            ]
        }
}
```
2) Create two PySpark dataframes from the JSON files:
### Stations table:
|row_num|internal_bus_station_id|public_bus_station|
|-|-|-|
|1|0|BAutogara|
|2|1|BVAutogara|
|3|2|SBAutogara|
|4|3|CJAutogara|
|5|4|MMAutogara|
|6|5|ISAutogara|
|7|6|CTAutogara|
|8|7|TMAutogara|
|9|8|BCAutogara|
|10|9|MSAutogara|

### Trips table:
|row_num|origin|destination|internal_bus_station_ids|triptimes|
|-|-|-|-|-|
|1|B|SB|[0, 2]|["2021-03-01 06:00:00", "2021-03-01 09:10:00"]|
|2|B|MM|[0, 2, 4]|["2021-03-01 10:10:00", "2021-03-01 12:20:10", "2021-03-01 14:10:10"]|
|3|BV|IS|[1, 8, 3, 5]|["2021-04-01 08:10:00", "2021-04-01 12:20:10", "2021-04-01 15:10:00", "2021-04-01 15:45:00"]|
|4|TM|CT|[7, 2, 9, 4, 6]|["2021-05-01 10:45:00", "2021-05-01 12:20:10", "2021-05-01 18:30:00", "2021-05-01 20:45:00", "2021-05-01 22:00:00"]|
|5|CJ|BC|[3, 9, 5, 6, 7, 8]|["2021-05-01 07:10:00", "2021-05-01 10:20:00", "2021-05-01 12:30:00", "2021-05-01 13:25:00", "2021-05-01 14:35:00", "2021-05-01 15:45:00"]||

3) Join the tables into a final Pyspark table that will replace the stations ids with the station names and also calculate the total trips duration (from origin to destination).

### Final table:
|row_num|origin|destination|public_bus_stops|trip_duration_total|
|-|-|-|-|-|
|1|B|SB|[BAutogara, SBAutogara]|190.0 min|
|2|B|MM|[BAutogara, SBAutogara, MMAutogara]|240.17 min|
|3|BV|IS|[BVAutogara, BCAutogara, CJAutogara, ISAutogara]|455.0 min|
|4|TM|CT|[TMAutogara, SBAutogara, MSAutogara, MMAutogara, CTAutogara]|675.0 min|
|5|CJ|BC|[CJAutogara, MSAutogara, ISAutogara, CTAutogara, TMAutogara, BCAutogara]|510.0 min|

