## answers to excercises_1.md

1. 
SELECT d.first_name, d.last_name, SUM(r.points) AS total_points
FROM race_results r
JOIN drivers d ON r.driver_id = d.driver_id
GROUP BY d.driver_id, d.first_name, d.last_name
HAVING SUM(r.points) > 0
ORDER BY total_points DESC;

-- the join is used to find frist and last names of the drivers, grouping by drivers, and then using having to clean the data and not include drivers with 0 points 

2. 
SELECT t.name AS team_name, COUNT(*) AS dnf_count
FROM race_results r
JOIN teams t ON r.team_id = t.team_id
WHERE r.status != 'Finished'
GROUP BY t.name
ORDER BY dnf_count DESC;

--the rows are filtered with WHERE to find out which teams did not finish, and then grouped to be ordered 

3. 
SELECT fanboost_used, AVG(final_position) AS avg_finish_position, COUNT(*) as total_instances
FROM race_results
WHERE final_position IS NOT NULL
GROUP BY fanboost_used;

--WHERE is used to clean out the data from DNFs so the average isnt fucked up, GROUP BY is used to seperate fanboosts that were and werent used 

4. 
SELECT d.last_name, l.lap_time_ms
FROM lap_times l
JOIN drivers d ON l.driver_id = d.driver_id
ORDER BY l.lap_time_ms ASC
LIMIT 1;

--i think this is pretty clear, order by shuffles the lap times from fastest to slowest and then limit 1 picks the first and highest one

5. 
SELECT d.first_name, d.last_name, r.grid_position, r.final_position, (r.grid_position - r.final_position) AS positions_gained
FROM race_results r
JOIN drivers d ON r.driver_id = d.driver_id
WHERE r.final_position IS NOT NULL
ORDER BY positions_gained DESC;

--ok this example sounded shit at first to me too, but its the same thing, just to show you that you can use math in SELECT. I hope i explained the excersise in the other file clear enough, if not sorry, i tried.

6. 
SELECT driver_id, points, CASE 
WHEN points >= 200 THEN 'competition'
WHEN points BETWEEN 150 AND 199 THEN 'middle'
ELSE 'loss'
END AS tier
FROM driver_standings;

7. 
SELECT d.last_name, r.grid_position, r.final_position
FROM drivers d
INNER JOIN race_results r ON d.driver_id = r.driver_id
WHERE r.grid_position < r.final_position;