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





