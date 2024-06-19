# <p align="center" style="margin-top: 0px;"> 🎮 When was the Golden Era of Video Games? 🏆

![video game](video%20game.png)

---

This report contains the solution of the SQL Project 'When was the Golden Era of Video Games?' available on DataCamp. [Click here](https://projects.datacamp.com/projects/2013) to access the complete project.

You can access the datasets given in [this file](Data) to practice on your own.  

# Introduction 
Video games are big business: the global gaming market is projected to be worth more than $300 billion by 2027 according to Mordor Intelligence. With so much money at stake, the major game publishers are hugely incentivized to create the next big hit. But are games getting better, or has the golden age of video games already passed?

In this project, you'll analyze video game critic and user scores as well as sales data for the top 400 video games released since 1977. You'll search for a golden age of video games by identifying release years that users and critics liked best, and you'll explore the business side of gaming by looking at game sales data.

Your search will involve joining datasets and comparing results with set theory. You'll also filter, group, and order data. Make sure you brush up on these skills before trying this project! The database contains two tables. Each table has been limited to 400 rows for this project, but you can find the complete dataset with over 13,000 games on Kaggle.

## Tables
1. `game_sales` table:

| Column       | Definition                        | Data Type |
|--------------|-----------------------------------|-----------|
| name         | Name of the video game            | varchar   |
| platform     | Gaming platform                   | varchar   |
| publisher    | Game publisher                    | varchar   |
| developer    | Game developer                    | varchar   |
| games_sold   | Number of copies sold (millions)  | float     |
| year         | Release year                      | int       |

The complete dataset of the `game_sales` table is available in [this file](game_sales.csv).


2. `reviews` table:

| Column       | Definition                                    | Data Type |
|--------------|-----------------------------------------------|-----------|
| name         | Name of the video game                        | varchar   |
| critic_score | Critic score according to Metacritic         | float     |
| user_score   | User score according to Metacritic           | float     |

The complete dataset of `reviews` table is available in [this file](reviews.csv).


3. `users_avg_year_rating` table:

| Column          | Definition                                           | Data Type |
|-----------------|------------------------------------------------------|-----------|
| year            | Release year of the games reviewed                    | int       |
| num_games       | Number of games released that year                    | int       |
| avg_user_score  | Average score of all the games ratings for the year  | float     |

The complete dataset of `users_avg_year_rating` table is available in [this file](users_avg_year_rating.csv).


4. `critics_avg_year_rating` table:

| Column           | Definition                                            | Data Type |
|------------------|-------------------------------------------------------|-----------|
| year             | Release year of the games reviewed                    | int       |
| num_games        | Number of games released that year                    | int       |
| avg_critic_score | Average score of all the games ratings for the year  | float     |

The complete dataset of `critics_avg_year_rating` is available in [this file](critics_avg_year_rating.csv).

---

# Questions:

##### 1. Find the ten best-selling games. The output should contain all the columns in the `game_sales` table and be sorted by the `games_sold` column in descending order.

## Query: 

```sql
SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10
```

## Output: 
| name                                     | platform | publisher         | developer         | games_sold | year |
|------------------------------------------|----------|-------------------|-------------------|------------|------|
| Wii Sports for Wii                      | Wii      | Nintendo          | Nintendo EAD      | 82.9       | 2006 |
| Super Mario Bros. for NES               | NES      | Nintendo          | Nintendo EAD      | 40.24      | 1985 |
| Counter-Strike: Global Offensive for PC | PC       | Valve             | Valve Corporation| 40         | 2012 |
| Mario Kart Wii for Wii                  | Wii      | Nintendo          | Nintendo EAD      | 37.32      | 2008 |
| PLAYERUNKNOWN'S BATTLEGROUNDS for PC    | PC       | PUBG Corporation  | PUBG Corporation  | 36.6       | 2017 |
| Minecraft for PC                        | PC       | Mojang            | Mojang AB         | 33.15      | 2010 |
| Wii Sports Resort for Wii               | Wii      | Nintendo          | Nintendo EAD      | 33.13      | 2009 |
| Pokemon Red / Green / Blue Version for GB| GB      | Nintendo          | Game Freak        | 31.38      | 1998 |
| New Super Mario Bros. for DS            | DS       | Nintendo          | Nintendo EAD      | 30.8       | 2006 |
| New Super Mario Bros. Wii for Wii       | Wii      | Nintendo          | Nintendo EAD      | 30.3       | 2009 |



##### 2. Find the ten years with the highest average critic score, where at least four games were released (to ensure a good sample size). Return an output with the columns year, `num_games` released, and `avg_critic_score`.<br> The `avg_critic_score` should be rounded to 2 decimal places. The table should be ordered by `avg_critic_score` in descending order.

## Query:

```sql
SELECT 
	year,
	num_games,
	ROUND(avg_critic_score,2) AS avg_critic_score
FROM critics_avg_year_rating
WHERE num_games >= 4
ORDER BY avg_critic_score DESC
LIMIT 10
```

## Output: 

| year | num_games | avg_critic_score |
|------|-----------|------------------|
| 1998 | 10        | 9.32             |
| 2004 | 11        | 9.03             |
| 2002 | 9         | 8.99             |
| 1999 | 11        | 8.93             |
| 2001 | 13        | 8.82             |
| 2011 | 26        | 8.76             |
| 2016 | 13        | 8.67             |
| 2013 | 18        | 8.66             |
| 2008 | 20        | 8.63             |
| 2017 | 13        | 8.62             |


##### 3. Find the years where critics and users broadly agreed that the games released were highly rated. Specifically, return the years where the average critic score was over 9 OR the average user score was over 9. <br><br>The pre-computed average critic and user scores per year are stored in `users_avg_year_rating` and `critics_avg_year_rating` tables respectively. The query should return the following columns: `year`, `num_games`, `avg_critic_score`, `avg_user_score`, and diff. <br><br>The diff column should be the difference between the `avg_critic_score` and `avg_user_score`. The table should be ordered by the year in ascending order.

## Approach:

* Select the `year`, `num_games`, `avg_critic_score` from `critics_avg_year_rating`(aliased as "cayr") and `avg_user_score` from `users_avg_year_rating` (aliased as "uayr"). 

* Subtract `uayr.avg_user_score` from `cayr.avg_critic_score` and alias it as `diff`

* INNER JOIN `users_avg_year_rating` using `year` as the foreign key.

* Using the `WHERE` clause filter the results where `cayr.avg_critic_score` OR `uayr.avg_user_score` is greater than 9. Order the results in ascending order of `year`. 

## Query:

```sql
SELECT 
    cayr.year,
    cayr.num_games,
    cayr.`avg_critic_score`,
    uayr.avg_user_score,
    cayr.avg_critic_score - uayr.avg_user_score AS diff
FROM critics_avg_year_rating cayr
INNER JOIN users_avg_year_rating uayr ON cayr.year = uayr.year
WHERE cayr.avg_critic_score > 9 OR uayr.avg_user_score > 9
ORDER BY cayr.year ASC;
```

## Output:

| year | num_games | avg_critic_score | avg_user_score | diff  |
|------|-----------|------------------|----------------|-------|
| 1997 | 8         | 7.93             | 9.5            | -1.57 |
| 1998 | 10        | 9.32             | 9.4            | -0.08 |
| 2004 | 11        | 9.03             | 8.55           | 0.48  |
| 2008 | 20        | 8.63             | 9.03           | -0.4  |
| 2009 | 20        | 8.55             | 9.18           | -0.63 |
| 2010 | 23        | 8.41             | 9.24           | -0.83 |



[def]: Data