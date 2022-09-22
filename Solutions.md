# Uber Trips Data Analysis:

- This repo contain business problem solution.

-  Open uber_tips.csv [here](https://github.com/Rakesh-Pawar/Uber-Trips-Data-Analysis/files/9624903/uber_tips.csv)
 
- Open SQL Data cleaning code [here](https://github.com/Rakesh-Pawar/Uber-Trips-Data-Analysis/blob/main/Data-cleaning.md)

# Solution:

## 1. How many trips have mr. Filipe Santos done over the years?

```SQL
select 
  year(cleen_Request_Time) as year, count(*) as trips_count
from 
  uber.cleaned_uber_trips
group by 1
order by 1;
```
**Step:**

- used `year()` function to fetch years from `cleen_Request_Time` then count all rows for counting total rides where done by Mr. filipe per year. 

**Answer:**

| **year**    | **trips_count** |
|:----------------------:|:----:|
| 2016                   | 17   |
| 2017                   | 120  |
| 2018                   | 148  |
| 2019                   | 111  |
| 2020                   | 51   |
| 2021                   | 2    |
 
 - From 2016 - 2021,total 449 rides where done by Mr. filipe. We can see that there is huge drop down aprox. -54% from 2019-2020 and aprox -95% from 2021-2022.

_____________________________________________________

## 2. What product type was usually chosen?

```SQL
select 
	product_type,
    count(product_type) as prod_count
from uber.cleaned_uber_trips
where product_type is not null
group by product_type
order by product_type desc;
```
**Step:**

- Use clean product type columns and counting them. We get ptoduct and total number of ride.

**Answer:**
| **product_type**          | **prod_count**|
|:-----------------------------:|:----:|
| UberX                         | 400  |
| Pool                          | 39   |
| Black                         | 1    |

- UberX is most frequently used by used about 91%. 
- Pool is used about 9%.
- Black used 1 %. 
______________________

## 3. What was the average fare, distance, amount, and time spent on rides?

```SQL
Select
	concat(round(avg(fare_amount),2), " Rs.") as avg_fare_amount,
	concat(round(avg(distance_km),2)," km") as avg_distance_km,
	concat(round(avg(fare_amount_km),2)," Rs/km") as avg_fare_amount_km,
	concat(round(avg(time_format((clean_Request_Time),"%i")),2)," minute") as avg_time_spend
from uber.cleaned_uber_trips;
```
**Step:**

- Use `fare_amount` column and used `avg` function to Calculate average, used `round` function for round decimal place up to 2.
- Used `concat` to join result as per unit. ex. currency Rs, distance km, time Minute.
- use `time_formate` to fetch time from `clean_Request_Time` used `"%i"` to return time in `Minute`

**Answer:**

| avg_fare_amount | avg_distance_km | avg_fare_amount_k | avg_time_spend |
|:-------------:|:-----------:|:--------------:|:-------------:|
| 19.77 Rs | 7.97 km | 8.89 Rs/km | 28 minute |

- User spend on an average 19.77 (20 Rs.) as fare amount per ride.
- User travels on an average 7.97 (8) km per ride.
- User spend on an average 9 Rs per km ride.
- User travels on an average 28 Minutes per ride.
_____________________________ 

## 4. What was the total fare, distance, amount, and time spent on rides?

```SQL
Select
	concat(round(sum(fare_amount),2), " Rs.") as total_fare_amount,
	concat(round(sum(distance_km),2)," km") as total_distance_km,
	concat(round(sum(fare_amount_km),2)," Rs/km") as total_fare_amount_km,
	concat(round(sum(time_format((clean_Request_Time),"%h")),2)," hours") as total_time_spend
from uber.cleaned_uber_trips;
```
**Step:**

- Use `fare_amount` column and used `sum` function to Calculate average, used `round` function for round decimal place up to 2.
- Used `concat` to join result as per unit. ex. currency Rs, distance km, time Minute.
- use `time_formate` to fetch time from `clean_Request_Time` used `"%h"` to return time in `hours`.

**Answer:**

| total_fare_amount | total_distance_km | total_fare_amount_km | total_time_spend |
|:-------------:|:-----------:|:--------------:|:-------------:|
| 8878.11 Rs. | 3576.65 km | 3990.84 Rs/km | 3965 hours |

- User spend 8878.11 Rs for all ride.
- User travels total distance 3576.65 km.
- User spend 3990.84 Rs/km.
- User travels total 3965 hours.
_________________________________

## 5. Which weekdays had the highest average fares per km ridden?

```SQL
select 	
	year(clean_Request_Time) as Year,
	round(avg(case when dayname(clean_Request_Time)='Monday'then fare_amount_km end),2) as Mon,
	round(avg(case when dayname(clean_Request_Time)='Tuesday'then fare_amount_km end),2) as Tue,
	round(avg(case when dayname(clean_Request_Time)='Wednesday'then fare_amount_km end),2) as Wed,
	round(avg(case when dayname(clean_Request_Time)='Thursday'then fare_amount_km end),2) as Thu,
	round(avg(case when dayname(clean_Request_Time)='Friday'then fare_amount_km end),2) as Fri,
	round(avg(case when dayname(clean_Request_Time)='Saturday'then fare_amount_km end),2) as Sat,
	round(avg(case when dayname(clean_Request_Time)='Sunday'then fare_amount_km end),2) as Sun
from uber.cleaned_uber_trips
group by 1
order by 1;
```
**Step:*

- used `year()` function to extract year from `clean_Request_Time`. Then used `dayname` function to extract weekdays. 
- Case when statement used to match the weekday and take average of `fare_amount_km`, then runded up to 2 decimal palces.

**Answer:**

| **Year** | **Mon** | **Tue** | **Wed** | **Thu** | **Fri** | **Sat** | **Sun** |
|:--------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
| 2016     | NULL    | NULL    | 7.23    | 6.32    | NULL    | 5.37    | 7.45    |
| 2017     | 8.36    | 6.87    | 7.33    | 8.01    | 9.66    | 8.49    | 7.94    |
| 2018     | 8.77    | 6.85    | 10.99   | 6.87    | 7.43    | 8.8     | 13.09   |
| 2019     | 8.28    | 7.05    | 14.56   | 9.05    | 9.81    | 9.8     | 16.68   |
| 2020     | 7.36    | 6.29    | 7.06    | 6.45    | 7.69    | 8.25    | 7.22    |
| 2021     | NULL    | NULL    | 6.32    | NULL    | NULL    | NULL    | 9.34    |

- According to the chart, we can see that Mondays, Wednesdays, Fridays and Sundays were on average the most expensive weekdays.
- Therefore, it allows us to better understand the weekly seasonality, and find out days with higher profitability for Uber and its drivers.
_____________________________________________________

## 6. Which was the longest/shortest and more expensive/cheaper ride?
```SQL
Select
	concat(max(distance_km), " km") as longest_ride,
	concat(min(distance_km)," km") as shortest_ride,
	concat(max(fare_amount_km)," Rs/km") as max_fare_amount_km,
	concat(min(fare_amount_km)," Rs/km") as min_fare_amount_km
from uber.cleaned_uber_trips;
```
**Step**
- Use `max()` to `distance_km` to calculate longest ride, use `concat` to join km to final result.
- Use `min()` to `distance_km` to calculate shortest ride.
- use `fare_amount_km` along with `max(), min()` function to calculate expensive and cheaper ride.
**Answer**

| **longest_ride** | **shortest_ride** | **max_fare_amount_km** | **min_fare_amount_km** |
|:----------------:|:-----------------:|:----------------------:|:----------------------:|
| 31.77 km         | 0.24 km           | 120.92 Rs/km           | 2.58 Rs/km             |

- User travels distance 31.77 km longest ride.
- User travels only 0.24 km shortest ride.
- User spend 121 Rs/km as maximum fare amount.
- User spend 2.60 Rs/km as minimum fare amount.
__________________

## 7. What was the average lead time before beginning a trip?

```SQL
select 
	concat(round(avg(lead_time),2)," minute") as avd_lead_time 
from(
	 select
        time_format(timediff(clean_Begin_Trip_Time,cleen_Request_Time),"%i") as lead_time
	from uber.cleaned_uber_trips) base;
```
**Step**
- Lead time calculated by taking difference between ride begin anf ride request time. For Example. If user booked UberX at 4.20 pm and driver pickup at 4.30 pm, this 10 min time called as lead time
- Use subquery to solve this problem. Use `timediff()` to calculate time difference between `clean_Begin_Trip_Time` and `cleen_Request_Time`.
- Use time_format to convert result in minute using "%i". name this column as lead_time.
- Use this `lead_time` column to calculate average time in main query.

**Answer**

| **avd_lead_time** |
|:-----------------:|
| 4.64 minute       |

- User spend 4.46 minute time between request time and begin time as lead time. 





























































