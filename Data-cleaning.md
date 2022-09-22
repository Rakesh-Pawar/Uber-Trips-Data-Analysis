- Open business Problem [Solution](Solution.md)
## Data Cleaning:

## Rename Product Type Columns 
```SQL
select * from (
select 
case
    when `Product Type` = 'UberX' then 'UberX'
    when `Product Type` = 'uberX' then 'UberX'
    when `Product Type` = 'uberX VIP' then 'UberX'
    when `Product Type` = 'VIP' then 'UberX'
    when `Product Type` = 'POOL' then 'Pool'
    when `Product Type` = 'POOL: MATCHED' then 'Pool'
    when `Product Type` = 'UberBLACK' then 'Black'
    when `Product Type` = 'uberx' then 'UberX'
    when `Product Type` = 'uberPOOL' then 'Pool'
    when `Product Type` = 'uberPOOL: MATCHED' then 'Pool'
    when `Product Type` = 'Pool: MATCHED' then 'Pool'
    else null
    
end as `Product Type`
FROM uber.uber_tips) a
where `Product Type` <> ' ' or `Product Type` is not null;
```
________________________________
## Cleaning Datetime column.
```SQL
select *,
year(cleen_Request_Time) as Request_year,
(monthname(cleen_Request_Time)) as Request_month,
(dayname(cleen_Request_Time)) as Request_day,
time_format(timediff(time(cleen_Begin_Trip_Time),time(cleen_Request_Time)),"%i") as request_lead_time,
time_format(timediff(time(cleen_Dropoff_Time),time(cleen_Begin_Trip_Time)),"%i") as trip_duration
from (
select 
cast(left(`Request Time`,position(' +0000 UTC' in `Request Time`)) as datetime) as cleen_Request_Time,
cast(left(`Begin Trip Time`,position(' +0000 UTC' in `Begin Trip Time`)) as datetime) as cleen_Begin_Trip_Time,
cast(left(`Dropoff Time`,position(' +0000 UTC' in `Dropoff Time`)) as datetime) as cleen_Dropoff_Time
from uber.uber_tips) base;
```
**Table:**

![datetime](https://user-images.githubusercontent.com/112051343/191700447-68818316-c0ad-4b0f-a944-64ccc071c3d0.JPG)
________________________________________________________________________________

## Creating new features

- rides['distance_km'] = round(rides.distance_miles*1.60934,2)
- rides['amount_km'] = round(rides.fare_amount/rides.distance_km,2)

```SQL
select round((`fare amount`/distance_km),2) as amount_km,distance_km from (
select `fare amount`,
round(`Distance (miles)`*1.60934,2) as distance_km
from uber.uber_tips) base;
```
**Table:**

![new_feature](https://user-images.githubusercontent.com/112051343/191700887-d922f9d6-7eb3-4200-937e-b89a25e258bc.JPG)

____________________________

## Creating Clean Table:

```SQL
create table uber.cleaned_uber_trips as(
select 
case 
    when `Product Type` = 'UberX' then 'UberX'
    when `Product Type` = 'uberX' then 'UberX'
    when `Product Type` = 'uberX VIP' then 'UberX'
    when `Product Type` = 'VIP' then 'UberX'
    when `Product Type` = 'POOL' then 'Pool'
    when `Product Type` = 'POOL: MATCHED' then 'Pool'
    when `Product Type` = 'UberBLACK' then 'Black'
    when `Product Type` = 'uberx' then 'UberX'
    when `Product Type` = 'uberPOOL' then 'Pool'
    when `Product Type` = 'uberPOOL: MATCHED' then 'Pool'
    when `Product Type` = 'Pool: MATCHED' then 'Pool'
    else null
end as Product_Type,
cast(left(`Request Time`,position(' +0000 UTC' in `Request Time`)) as datetime) as cleen_Request_Time,
cast(left(`Begin Trip Time`,position(' +0000 UTC' in `Begin Trip Time`)) as datetime) as cleen_Begin_Trip_Time,
cast(left(`Dropoff Time`,position(' +0000 UTC' in `Dropoff Time`)) as datetime) as cleen_Dropoff_Time,
`Begin Trip Lat` as begin_trip_lat,
 `Begin Trip Lng` as begin_trip_lng,
`Dropoff Lat` as dropoff_lat,
`Dropoff Lng` as dropoff_lng,
`fare amount` as fare_amount,
round(`Distance (miles)`*1.60934,2) as distance_km,
round((`fare amount`/`Distance (miles)`*1.60934),2) as fare_amount_km 
FROM uber.uber_tips);
```
**Table**

![clean data](https://user-images.githubusercontent.com/112051343/191701174-73e62326-1083-4896-848c-a796d59f8419.JPG)


- Here I am not creating Year, Month, Day columns. I used year(), monthnam() function to fetch related data in query.
