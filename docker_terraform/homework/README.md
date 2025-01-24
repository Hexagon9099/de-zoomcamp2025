# Q1 Run docker with the python:3.12.8 image in an interactive mode, use the entrypoint bash. What's the version of pip in the image?

$ docker run -it --entrypoint=bash python:3.12.8 \
$ pip --version \
pip 24.3.1 

# Q2 Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?

db:5432

# Q3 Prepare Postgres
$ docker-compose up -d #using docker-compose.yaml file
# During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, respectively, happened:
# Up to 1 mile 
# In between 1 (exclusive) and 3 miles (inclusive),
# In between 3 (exclusive) and 7 miles (inclusive),
# In between 7 (exclusive) and 10 miles (inclusive),
# Over 10 miles
SELECT
	CASE
		WHEN trip_distance <=1 THEN '(1) up to 1 mile'
		WHEN trip_distance >1 AND trip_distance <=3 THEN '(2) 1-3 miles'
		WHEN trip_distance >3 AND trip_distance <=7 THEN '(3) 3-7 miles'
		WHEN trip_distance >7 AND trip_distance <=10 THEN '(4) 7-10 miles'
		WHEN trip_distance >10 THEN '(5) over 10 miles'
	END AS distance_range,
	COUNT (1) AS trips
FROM taxi_trips_10_2019
GROUP BY distance_range
ORDER BY distance_range
