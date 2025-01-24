# Q1 Run docker with the python:3.12.8 image in an interactive mode, use the entrypoint bash. What's the version of pip in the image?

$ docker run -it --entrypoint=bash python:3.12.8 \
$ pip --version \
pip 24.3.1 


# Q2 Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?

db:5432


# Q3 During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, respectively, happened:
Up to 1 mile \
In between 1 (exclusive) and 3 miles (inclusive), \
In between 3 (exclusive) and 7 miles (inclusive), \
In between 7 (exclusive) and 10 miles (inclusive), \
Over 10 miles 

SELECT \
	CASE \
		WHEN trip_distance <=1 THEN '(1) up to 1 mile' \
		WHEN trip_distance >1 AND trip_distance <=3 THEN '(2) 1-3 miles' \
		WHEN trip_distance >3 AND trip_distance <=7 THEN '(3) 3-7 miles' \
		WHEN trip_distance >7 AND trip_distance <=10 THEN '(4) 7-10 miles' \
		WHEN trip_distance >10 THEN '(5) over 10 miles' \
	END AS distance_range, \
	COUNT (1) AS trips \
FROM taxi_trips_10_2019 \
GROUP BY distance_range \
ORDER BY distance_range


# Q4 Which was the pick up day with the longest trip distance? Use the pick up time for your calculations.

SELECT \
	CAST (lpep_pickup_datetime AS DATE) AS day, \
	MAX (trip_distance) AS distance \
FROM taxi_trips_10_2019 \
GROUP BY day \
ORDER BY distance DESC \


# Q5 Which were the top pickup locations with over 13,000 in total_amount (across all trips) for 2019-10-18?

SELECT \
	CAST (lpep_pickup_datetime AS DATE) AS day, \
	SUM (total_amount) AS sum, \
	z."Zone" \
FROM taxi_trips_10_2019 t \
JOIN zones_homework z ON t."PULocationID" = z."LocationID" \
WHERE CAST (lpep_pickup_datetime AS DATE) = '2019-10-18' \
GROUP BY z."Zone", CAST (lpep_pickup_datetime AS DATE) \
ORDER BY sum DESC \
LIMIT 100


# Q6 For the passengers picked up in October 2019 in the zone named "East Harlem North" which was the drop off zone that had the largest tip?

SELECT \
	z_dropoff."Zone" AS dropoff_zone, \
	MAX (t.tip_amount) tip \
FROM taxi_trips_10_2019 t \
JOIN zones_homework z_pickup ON t."PULocationID" = z_pickup."LocationID" \
JOIN zones_homework z_dropoff ON t."DOLocationID" = z_dropoff."LocationID" \
WHERE \
	CAST(t.lpep_pickup_datetime AS DATE) BETWEEN '2019-10-01' AND '2019-10-31' \
	AND z_pickup."Zone"='East Harlem North' \
GROUP BY z_dropoff."Zone" \
ORDER BY tip DESC \
LIMIT 1
