# Subquery where not exists + row number window function

Given that one cannot have overlapping availabilities for a single accommodation;
return all visits that fall outside of an availability, with the lastest availability preceeding the visit.

In other word; if you have a visit from 2023-10-10 10:00:00Z to 2023-10-10 11:00:00Z;
But the accommodation's availabilities are:
  - from 2023-09-20 10:00:00Z to 2023-09-20 18:00:00Z
  - from 2023-10-05 15:00:00Z to 2023-10-05 21:00:00Z

Return the availability from 2023-10-05 15:00:00Z to 2023-10-05 21:00:00Z.

Here's the example setup:
```sql
DROP TABLE IF EXISTS Availabilities;
DROP TABLE IF EXISTS Visits;
DROP TABLE IF EXISTS Accommodations;

CREATE TABLE IF NOT EXISTS Accommodations (
	id INT PRIMARY KEY
);

CREATE TABLE IF NOT EXISTS Visits (
  id INT PRIMARY KEY,
	accommodation_id INT,
  FOREIGN KEY (accommodation_id) REFERENCES Accommodations(id),
	start_datetime TIMESTAMP, 
	end_datetime TIMESTAMP
);

CREATE TABLE IF NOT EXISTS Availabilities (
	id INT PRIMARY KEY,
	accommodation_id INT,
  FOREIGN KEY (accommodation_id) REFERENCES Accommodations(id),
	start_datetime TIMESTAMP,
  end_datetime TIMESTAMP
);
 

-- Inserting into Accommodations
INSERT INTO Accommodations (id)
VALUES (1), (2), (3);

-- Inserting into Visits
INSERT INTO Visits (id, accommodation_id, start_datetime, end_datetime)
VALUES 
  (1, 1, '2023-09-20T10:00:00Z', '2023-09-20T12:00:00Z'),
  (2, 1, '2023-12-20T10:00:00Z', '2023-12-20T12:00:00Z'),
  (3, 1, '2023-09-21T14:00:00Z', '2023-09-21T16:00:00Z'),
  (4, 2, '2023-09-22T11:00:00Z', '2023-09-22T13:00:00Z'),
  (5, 2, '2023-09-23T15:00:00Z', '2023-09-23T17:00:00Z'),
  (6, 2, '2024-01-01T10:00:00Z', '2024-01-01T0:45:00Z'),
  (7, 2, '2020-10-23T20:00:00Z', '2020-10-23T21:00:00Z'),
  (8, 3, '2023-09-24T09:00:00Z', '2023-09-24T11:00:00Z');

-- Inserting into Availabilities
INSERT INTO Availabilities (id, accommodation_id, start_datetime, end_datetime)
VALUES
  (1, 1, '2023-09-18T08:00:00Z', '2023-09-30T09:30:00Z'),
  (2, 1, '2023-09-10T13:00:00Z', '2023-09-11T06:00:00Z'),
  (3, 2, '2023-09-22T10:00:00Z', NULL),
  (4, 2, '2023-09-23T14:00:00Z', '2023-09-23T16:00:00Z'),
  (5, 3, '2023-09-24T08:00:00Z', NULL),
  (6, 3, NULL, NULL);
```

First, a very basic query to return all of the visits associated to an accommodations; and all the accommodations associated to an availability, can be written like so:

```sql
SELECT
  accommodations.*, 
	availabilities.*, 
	visits.*
FROM visits
JOIN accommodations ON visits.accommodation_id = accommodations.id 
JOIN availabilities ON availabilities.accommodation_id = accommodations.id
ORDER BY visits.id, availabilities.end_datetime DESC
```

The above query returns visits that are within an availability; which we do not want.
To ensure that only visits outside of an availability are returned, we can do this:

```sql
SELECT
	accommodations.*, 
	availabilities.*, 
	visits.*
FROM visits
JOIN accommodations ON visits.accommodation_id = accommodations.id 
JOIN availabilities ON availabilities.accommodation_id = accommodations.id
AND NOT EXISTS (
  SELECT 1
  FROM availabilities
  WHERE availabilities.accommodation_id = accommodations.id
  AND visits.start_datetime >= availabilities.start_datetime
  AND visits.end_datetime <= COALESCE(availabilities.end_datetime, now())
)
ORDER BY visits.id, availabilities.end_datetime DESC
```

However, it returns all visits' accommodations' availabilities; even though we just want the visits and their latest preceeding availability.
To achieve this, we can use `ROW_NUMBER()` to each visits, in descending order by the accommmodation's availabilities' end_datetime, and then select for each windows' first row:

```sql
WITH ordered_availabilities AS (
	SELECT
		accommodations.*,
		availabilities.*,
		visits.*,
		ROW_NUMBER() OVER (
			PARTITION BY visits.id
			ORDER BY availabilities.end_datetime DESC
		)
	FROM visits
	JOIN accommodations ON visits.accommodation_id = accommodations.id 
	JOIN availabilities ON availabilities.accommodation_id = accommodations.id
	WHERE NOT EXISTS (
		SELECT 1
		FROM availabilities
		WHERE availabilities.accommodation_id = accommodations.id
		AND visits.start_datetime >= availabilities.start_datetime 
		AND visits.end_datetime <= COALESCE(availabilities.end_datetime, now())
	)
)
SELECT *
FROM ordered_availabilities
WHERE row_number = 1
ORDER BY 1
```