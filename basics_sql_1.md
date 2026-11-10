# Basic commands explained cheatsheet SQL

## 5 most important basic commands and what they do
- SELECT : Which column do you want to look into?
- FROM : Which table is it inside of?
- WHERE : Which row matches your conditions?
- ORDER BY : How do you want the rows to be sorted?
- LIMIT : How many rows do you want to output?

## SELECT - used to choose columns 
Function + example 

SELECT * FROM teams;
-- this selects all columns, avoid if possible in professional settings especially interviews, in testing ok

SELECT first_name, last_name, age FROM drivers;
-- this selects specific columns from a table, safe option in production and interviews and ALWAYS

## WHERE - used to filter rows for needed information 
examples explained:

SELECT first_name, last_name, country
FROM drivers 
WHERE country = 'Germany';

-- in this case the = sign is used to filter data so only data that matches this specific filter will output.
-- you can use as well : 
-- =! 
-- <>
-- < 
-- > 
-- <=
-- >= 

example:

SELECT first_name, last_name, age
FROM drivers
WHERE age < 40;

--this will filter out drivers that are older than 40 and only output drivers younger.

## WHERE combined with conditions : AND/OR
example:

SELECT first_name, last_name, country, age
FROM drivers
WHERE country = 'United States' AND age <= 30;

--this will output all drivers that from the US and are younger or 30 years old.

## WHERE with LIKE - for very specific filtering

1. filtering by prefixes 

SELECT first_name
FROM drivers
WHERE first_name LIKE 'P%';

--will output all first names starting with the letter P

2. filtering by suffixes 

SELECT race_name
FROM races
WHERE status LIKE '%ePrix';

--will output all race names that end with ePrix

3. filtering by specific words

SELECT circuit_name 
FROM races
WHERE circuit_name LIKE '%Street%';

--will select all circuit names that have Street in them 

4. filtering by length (very rare)

SELECT first_name 
FROM drivers
WHERE first_name LIKE '___';

--will select all first names that have exactly 3 letters

5. mixing both % and -

SELECT last_name 
FROM drivers 
WHERE last_name LIKE '_o%';

-- will output all last names that have an 'o' as the second letter

SELECT last_name 
FROM drivers 
WHERE last_name LIKE 'K_n%';

--will output all last names that have 'K' as first letter, 'n' as third. e.g. 'King'

6. reverse LIKE - NOT LIKE 

SELECT circuit_name 
FROM races
WHERE circuit_name NOT LIKE '%Street%';

-- will output all circuit names that do NOT have the word Streets in them.

7. also can combine with AND/OR

SELECT last_name 
FROM drivers 
WHERE last_name LIKE 'K_n%' OR last_name LIKE '%n';

## IN - basically OR OR but cleaner and faster 

WHERE country = 'United States' OR country = 'UK' OR country = 'France';

WHERE country IN ('United States', 'UK', 'France')

- can be reversed = NOT IN 

WHERE country NOT IN ('United States', 'UK', 'France')

--will output the drivers that are not from US, UK and France instead
--this function is important, might come up in interviews because of easy mistakes

## BETWEEN - filtering by 2 values, from - to

example:

SELECT points
FROM driver_standings
WHERE points BETWEEN 150 AND 250;

--will output the points of drivers only in between 150 - 250

## ORDER BY - used to sort data 

example for increasing order sorting 

SELECT season_id, driver_id, points
FROM driver_standings 
ORDER BY points;

-- will output points from lowest to highest 

example for decreasing order sorting 

SELECT season_id, driver_id, points
FROM driver_standings 
ORDER BY points DESC;

--will output the points from highest to lowest 

example for sorting multiple columns 

SELECT season_id, driver_id, points
FROM driver_standings 
ORDER BY driver_id ASC, points DESC;

-- will output the driver ids from low to high and points from high to low

## LIMIT - used to set a_ limit for searching data 

SELECT season_id, driver_id, points 
FROM driver_standings
ORDER BY points 
LIMIT 5;

--will output 5 top points

## DISTINCT - used to remove duplicats

SELECT DISTINCT season_id
FROM driver_standings

--will output only unique values in random order  

## DROP - delete tables or columns 

deleting a specific column

ALTER TABLE drivers
DROP COLUMN driver_id;

--will delete column driver_id from table drivers 

OR

ALTER TABLE drivers
DROP driver_id;

--should also delete specific column

DROP TABLE IF EXISTS drivers;

--checks if table exists and if it does it deletes it completely, use wisely

## CREATING TABLES

example

CREATE TABLE table_name (
    name TEXT NOT NULL UNIQUE,
    age INTEGER,
    id PRIMARY KEY AUTOINCREMENT,
    nickname TEXT (3)
    year NOT NULL INTEGER
);

-- im skipping the rest of basics basics cause they are known, if not lmk I will edit and add them.

# Data analysis functions 
## Aggregates 
basically how to read and summarize multiple rows at once instead of one by one 

-- main aggregate functions 
COUNT() - counts how many rows 
SUM() - total 
AVG() - average
MIN()/MAX() - minimum/maximum 

-- GROUP BY - needed to use aggregate functions - takes all the rows with the same value and puts them together so calculations are possible 


example
-- outputs total amount of rows in race_results 
SELECT COUNT(*) FROM race_results;

--outputs how many results are there for each status
SELECT status, COUNT(*) AS total_occurrences
FROM race_results
GROUP BY status;

SELECT SUM(points) AS total_points_awarded, AVG(points) AS avg_points_per_result, COUNT(*) AS total_results
FROM race_results;

--AS function is used for stylistic output purposes so it looks better 

category group example:

SELECT status, 
COUNT(*) AS occurrences
FROM race_results
GROUP BY status
ORDER BY occurrences DESC;

lastly grouping and filtering at the same time expamle:

SELECT driver_id, SUM(points) AS total_points, COUNT(*) AS races
FROM race_results
GROUP BY driver_id
HAVING COUNT(*) > 2; 

--REMEMBER - GROUP BY
when using this function, every column you want to use has to use either a category you are grouping by OR have a calculation function like COUNT(), SUM(), MAX()...

example: if you put status and final_position in SELECT, you need to also put it in GROUP BY.

-- wrong
SELECT driver_id, status, SUM(points) FROM race_results GROUP BY driver_id;

-- good
SELECT driver_id, status, SUM(points) FROM race_results GROUP BY driver_id, status;

## HAVING
this is used to filter groups after using aggregates. 
REMEBER:
WHERE filters rows before they are grouped.

example:
SELECT driver_id, AVG(grid_position)
FROM race_results
GROUP BY driver_id
HAVING COUNT(*) > 3;  

--outputs the groups that have 4 or more rows

having and where together example:

SELECT driver_id, SUM(points) AS total_points
FROM race_results
WHERE status = 'Finished'   
GROUP BY driver_id
HAVING SUM(points) > 50;

again, where filters rows, having filters groups !!!!

--IMPORTANT

aggregate functions IGNORE NULL values

COUNT(column) will ignore NULL, BUT COUNT(*) will count EVERy ROW
if you have a situation : AVG(points) and three rows have NULL for points, AVG is output by dividing by the rows that have points. NULL is not seen as a 0 value!!!!


## JOINS - combining tables 

most used and IMPORTANT JOINS:

INNER JOIN - outputs only rows with matches in both tables

LEFT JOIN - outputs all rows from the left table, and the matched rows from the right table. if the right side has no match then it will return a NULL value.

ON condition - used to tell SQL how the tables relate to eachother, REMEMBER IT, if not = CARTESIAN PRODUCT, you will have unlimited bullshit output
example:

ON r.driver_id = d.driver_id


INNER JOIN example:

SELECT d.first_name, d.last_name, r.points
FROM race_results r
JOIN drivers d ON r.driver_id = d.driver_id;

--will output only drivers who have raced 

LEFT JOIN example:

SELECT d.first_name, d.last_name, r.points
FROM drivers d
LEFT JOIN race_results r ON d.driver_id = r.driver_id;

--will output ALL drivers even if they havent raced yet


## PRAGMA CHEATSHEET
 
 MOST IMPORTANT PRAGMA function - ALWAYS REMEMBER SQLite :

 PRAGMA foreign_keys = ON; 

 -- the rest of PRAGMA functions and what they do

 PRAGMA table_info(race_results);
-- shows you wtf you have in your table (column ID, type, name, NULL structure, default values and primary key status)- useful for debugging

PRAGMA foreign_key_list(race_results);
-- shows you all foreign keys for race_results

PRAGMA index_list(race_results);
-- shows all indexes in race_results - useful for query optimization

PRAGMA table_list;

OR

SELECT name FROM sqlite_master WHERE type='table';
--shows all tables in database 

# Differences between SQLite and PostgreSQL that I know of (there might be more so disclaimer)

                           SQLite                            PostgreSQL

DATA TYPES                stores anything in                will fail if you try to store a different data type than declared 
                            a column                        in the column like STR in an INT column.


AUTOINCREMENT      INTEGER PRIMARY KEY AUTOINCREMENT         SERIAL

CASE SENSITIVE              no, unless (' ')                 yes (LIKE - case insensitive SQLite, ILIKE - case sensitive)

BOOLEANS                 binary - (0, 1)                     TRUE/ FALSE

DATE/TIME                  TEXT or INT                       DATE/TIMESTAMP/INTERVAL

UPDATE/DELETE         um... choose a god and pray            RETURNING  *
                       if its a modified column 


