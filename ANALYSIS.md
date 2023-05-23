# Analysis


## SQL Server

```sql
WITH hotels AS (
	SELECT * FROM dbo.['2018$']
	UNION
	SELECT * FROM dbo.['2019$']
	UNION
	SELECT * FROM dbo.['2020$']
)
```

### Is our Hotel data growing

#### By year

```sql
SELECT 
	arrival_date_year,
	ROUND(SUM((stays_in_week_nights + stays_in_weekend_nights) * adr), 2) AS revenue
FROM hotels
GROUP BY arrival_date_year
```

arrival_date_year | revenue
-- | --
2018 | 4885517.06
2019 | 20188409.4
2020 | 14284246.24

#### By year and hotel type

```sql
SELECT 
	arrival_date_year,
	hotel,
	ROUND(SUM((stays_in_week_nights + stays_in_weekend_nights) * adr), 2) AS revenue
FROM hotels
GROUP BY 
    arrival_date_year,
    hotel
```

arrival_date_year | hotel | revenue
-- | -- | --
2018 | City Hotel | 1764667.57
2019 | City Hotel | 10755979.11
2020 | City Hotel | 8018122.43
2018 | Resort Hotel | 3120849.49
2019 | Resort Hotel | 9432430.29
2020 | Resort Hotel | 6266123.81

## Power BI

### Importing sql data in power BI

Import the SQL Server by using connection name and database name. Select Advanced Options and enter the following SQL Query

```sql
WITH hotels AS (
	SELECT * FROM dbo.['2018$']
	UNION
	SELECT * FROM dbo.['2019$']
	UNION
	SELECT * FROM dbo.['2020$']
)

SELECT * FROM hotels
	LEFT JOIN dbo.market_segment$
	ON hotels.market_segment = market_segment$.market_segment
	LEFT JOIN dbo.meal_cost$
	ON meal_cost$.meal = hotels.meal
```
