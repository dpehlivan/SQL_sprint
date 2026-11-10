## Exercises for fe_2.db 

1. try to find the drivers first and last names, with their summed points. include only drivers that have scored points. and after you find them, sort the data from the winner (highest points), to the loser (lowest).

2. find the teams that have DNFs (did not finish), and how many of their races have ended in a DNF. then grouop them by team and try to order them by their count DESC.

3. try to find the average for final_position, for results where 1 fanboost was used, and then try to compare it against the average where the fanboost used is equal to 0. 

4. use join for lap_times and drivers tables and output the best drivers last name alongside his laptime. hint: after joining tables order laptimes ASC and use LIMIT.

HARDER EXAMPLE:

5. find the first, last names and the differences between grid_position and final_position. (if they started 10th and finished 5th, the difference is 5 positions) - math is allowed in SELECT

EXERCISE WITH CASE

6. use driver_standings and try to write code to show the driver_id and their points. use case to create a column "tier". if the points are bigger or equal to 200 call them "competition", if the points are between 150 and 199, call them "middle", label the rest as "loss".

INNER JOIN

7. combine drivers and race_results and show last_name, grid_position, final_position. only return results where grid position (lower) was better than final position (position loss).
