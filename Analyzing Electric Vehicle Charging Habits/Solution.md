
# <p align="center" style="margin-top: 0px;"> 🔋 Analyzing Electric Vehicle Charging Habits 🚗

![dollar bill](car%20charging.png)

---
This report contains the solution of the SQL Project 'Analyzing Electric Vehicle Charging Habits' available on DataCamp. To access the complete project click on this [link]("https://projects.datacamp.com/projects/2408").

# Introduction
As electronic vehicles (EVs) become more popular, there is an increasing need for access to charging stations, also known as ports. To that end, many modern apartment buildings have begun retrofitting their parking garages to include shared charging stations. A charging station is shared if it is accessible by anyone in the building.

But with increasing demand comes competition for these ports — nothing is more frustrating than coming home to find no charging stations are available! In this project, you will use a dataset to help apartment building managers better understand their tenants’ EV charging habits. The following questions will be answered:

* The number of unique individuals that use each garage’s shared charging stations.
* The top 10 most popular charging start times for sessions that use shared charging stations.
* Users whose average charging sessions last longer than 10 hours when using shared charging stations. 

Access has been granted to the `charging_sessions` table, which is as follows: 

| Column | Definition | Data type |
|-|-|-|
|`garage_id`| Identifier for the garage/building|`VARCHAR`|
|`user_id` | Identifier for the individual user|`VARCHAR`|
|`user_type`|Indicating whether the station is `Shared` or `Private`| `VARCHAR` |
|`start_plugin`|The date and time the session started |`DATETIME`|
|`start_plugin_hour`|The hour (in military time) that the session started | `NUMERIC`|
|`end_plugout`|The date and time the session ended | `DATETIME` |
|`eng_plugin_hour`|The hour (in military time) that the session ended | `NUMERIC`|
|`duration_hours`| The length of the session, in hours|`NUMERIC`|
|`el_kwh`| Amount of electricity used (in Kilowatt hours)|`NUMERIC`|
|`month_plugin`| The month that the session started |`VARCHAR`|
|`weekdays_plugin`| The day of the week that the session started|`VARCHAR`|

The full table is provided in [this csv file](charging_sessions.csv)

First, we will take a look at the data in hand by selecting all the columns. Limiting the data to 10 rows to keep the output clean.
### Query

```sql
SELECT *
FROM charging_sessions
LIMIT 10
```

### Output
| garage_id | user_id | user_type | start_plugin         | start_plugin_hour | end_plugout          | end_plugout_hour | el_kwh | duration_hours | month_plugin | weekdays_plugin |
|-----------|---------|-----------|----------------------|-------------------|----------------------|------------------|--------|----------------|--------------|----------------|
| AdO3      | AdO3-4  | Private   | 2018-12-21T10:20:00  | 10                | 2018-12-21T10:23:00  | 10               | 0.3    | 0.05           | Dec          | Friday         |
| AdO3      | AdO3-4  | Private   | 2018-12-21T10:24:00  | 10                | 2018-12-21T10:32:00  | 10               | 0.87   | 0.136666667    | Dec          | Friday         |
| AdO3      | AdO3-4  | Private   | 2018-12-21T11:33:00  | 11                | 2018-12-21T19:46:00  | 19               | 29.87  | 8.216388889    | Dec          | Friday         |
| AdO3      | AdO3-2  | Private   | 2018-12-22T16:15:00  | 16                | 2018-12-23T16:40:00  | 16               | 15.56  | 24.41972222    | Dec          | Saturday       |
| AdO3      | AdO3-2  | Private   | 2018-12-24T22:03:00  | 22                | 2018-12-24T23:02:00  | 23               | 3.62   | 0.970555556    | Dec          | Monday         |
| AdO3      | AdO3-2  | Private   | 2018-12-24T23:32:00  | 23                | 2018-12-25T17:37:00  | 17               | 16.14  | 18.07777778    | Dec          | Monday         |
| AdO3      | AdO3-2  | Private   | 2018-12-25T18:25:00  | 18                | 2018-12-26T16:08:00  | 16               | 10.33  | 21.72083333    | Dec          | Tuesday        |
| AdO3      | AdO3-4  | Private   | 2018-12-26T10:41:00  | 10                | 2018-12-26T16:52:00  | 16               | 27.66  | 6.188055556    | Dec          | Wednesday      |
| AdO3      | AdO3-2  | Private   | 2018-12-26T18:46:00  | 18                | 2018-12-26T21:08:00  | 21               | 8.83   | 2.371111111    | Dec          | Wednesday      |
| AdO3      | AdO3-2  | Private   | 2018-12-29T16:04:00  | 16                | 2018-12-29T20:55:00  | 20               | 8.58   | 4.856111111    | Dec          | Saturday       |


---
# Questions

## 1. What is the number of unique individuals that use each garage’s shared charging stations?
* The output should contain two columns: `garage_id` and `unique_users`. Sort your result from most to least unique users.

**Approach:**
* Select the `garage_id`, count the _distinct_ `user_id`s and alias it as `unique_users`. 
* Use the `WHERE` clause to select only the _Shared_ `user_type`s. Group the results by `garage_id` and Order the result in descending order of `unique_users`. 

### Query: 

```sql
SELECT 
	garage_id, 
	COUNT(DISTINCT user_id) AS unique_users
FROM public.charging_sessions
WHERE user_type = 'Shared'
GROUP BY garage_id
ORDER BY unique_users DESC
```

### Output:
| garage_id | unique_users |
|-----------|--------------|
| Bl2       | 18           |
| AsO2      | 17           |
| UT9       | 16           |
| AdO3      | 3            |
| MS1       | 2            |
| SR2       | 2            |
| AdA1      | 1            |
| Ris       | 1            |


## 2. What are the top 10 most popular charging start times for sessions that use shared charging stations?
* The output should contain three columns: `weekdays_plugin`, `start_plugin_hour`, and a column named `num_charges` containing the number of plugins on that weekday at that hour. The results should be sorted from the most to the least number of sessions.

**Approach:**
* Select the `weekdays_plugin`, `start_plugin_hour` columns and count all rows as `num_charges`. Use the `WHERE` clause to select only those rows where the `user_type` is _Shared_.
* Group the results by `weekdays_plugin` and `start_plugin_hour`. Order it in descending order of the number of charges and limit the rows to 10. 

### Query: 

```sql
SELECT weekdays_plugin,
	start_plugin_hour, 
	COUNT(*) AS num_charges
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY weekdays_plugin,
	start_plugin_hour
ORDER BY num_charges DESC
LIMIT 10;
```

### Output: 
| weekdays_plugin | start_plugin_hour | num_charges |
|-----------------|-------------------|-------------|
| Sunday          | 17                | 30          |
| Friday          | 15                | 28          |
| Thursday        | 19                | 26          |
| Thursday        | 16                | 26          |
| Wednesday       | 19                | 25          |
| Sunday          | 18                | 25          |
| Sunday          | 15                | 25          |
| Monday          | 15                | 24          |
| Friday          | 16                | 24          |
| Tuesday         | 16                | 23          |



## 3. How many users whose average charging sessions last longer than 10 hours when using shared charging stations?
* The output should contain two columns: `user_id` and `avg_charging_duration`. The results should be sorted from highest to lowest average charging duration.

**Approach:** 
* SELECT the `user_id` and average of `duration_hours`, and alias it as `avg_charging_duration`. Filter the results to only include _shared_ stations. 
* Group the columns by `user_id` and keep only those whose average charging duration is more than 10. (Using the `HAVING` clause)
* Order the results in `DESC` order of average charging duration. 

### Query: 
```sql
SELECT user_id, 
	AVG(duration_hours) as avg_charging_duration
FROM charging_sessions 
WHERE user_type = 'Shared' 
GROUP BY user_id
HAVING AVG(duration_hours) > 10
ORDER BY AVG(duration_hours) DESC;
```

### Output: 
| user_id | avg_charging_duration |
|---------|-----------------------|
| Share-9 | 16.845833335          |
| Share-17| 12.8945555511         |
| Share-25| 12.2144747466         |
| Share-18| 12.0888071898         |
| Share-8 | 11.5504308392         |
| AdO3-1  | 10.3693869729         |

---