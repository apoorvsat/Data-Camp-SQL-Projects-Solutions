# <p align="center" style="margin-top: 0px;"> ⚕️ Analyzing Students' Mental Health 🧠

![mental health pic](mental.png)

---
This report contains the solution of the SQL Project 'Analyzing Students' Mental Health' available on DataCamp. To access the complete project click on this [link](https://projects.datacamp.com/projects/1593).

# Introduction
Does going to university in a different country affect your mental health? A Japanese international university surveyed its students in 2018 and published a study the following year that was approved by several ethical and regulatory boards.

The study found that international students have a higher risk of mental health difficulties than the general population, and that social connectedness (belonging to a social group) and acculturative stress (stress associated with joining a new culture) are predictive of depression.

Explore the [students](students.csv) data using PostgreSQL to find out if you would come to a similar conclusion for international students and see if the length of stay is a contributing factor.

Access has been granted to the `students` data which is as follows:

| Field Name    | Description                                      |
| ------------- | ------------------------------------------------ |
| `inter_dom`     | Types of students (international or domestic)   |
| `japanese_cate` | Japanese language proficiency                    |
| `english_cate`  | English language proficiency                     |
| `academic`      | Current academic level (undergraduate or graduate) |
| `age`           | Current age of student                           |
| `stay`          | Current length of stay in years                  |
| `todep`         | Total score of depression (PHQ-9 test)           |
| `tosc`          | Total score of social connectedness (SCS test)   |
| `toas`          | Total score of acculturative stress (ASISS test) |

The full table is provided in [this csv file](students.csv)

First, we will take a look at the data in hand by selecting all the columns. Limiting the data to 5 rows to keep the output clean.

```sql
SELECT *
FROM students
LIMIT 10
```

Output:
The dataset includes more columns other than the ones defined above. The results of this query can be viewed [here](first%205%20rows.csv).


---
# Project Instructions: 

* Return a table with nine rows and five columns.
* The five columns should be aliased as: stay, count_int, average_phq, average_scs, and average_as, in that order.
* The average columns should contain the average of the todep (PHQ-9 test), tosc (SCS test), and toas (ASISS test) columns for each length of stay, rounded to two decimal places.
The count_int column should be the number of international students for each length of stay.
* Sort the results by the length of stay in descending order.

## Query: 
```sql
SELECT stay, 
       COUNT(*) AS count_int,
       ROUND(AVG(todep), 2) AS average_phq, 
       ROUND(AVG(tosc), 2) AS average_scs, 
       ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

## Output: 
| stay | count_int | average_phq | average_scs | average_as |
|------|-----------|-------------|-------------|------------|
| 10   | 1         | 13          | 32          | 50         |
| 8    | 1         | 10          | 44          | 65         |
| 7    | 1         | 4           | 48          | 45         |
| 6    | 3         | 6           | 38          | 58.67      |
| 5    | 1         | 0           | 34          | 91         |
| 4    | 14        | 8.57        | 33.93       | 87.71      |
| 3    | 46        | 9.09        | 37.13       | 78         |
| 2    | 39        | 8.28        | 37.08       | 77.67      |
| 1    | 95        | 7.48        | 38.11       | 72.8       |

# Conclusion 
The data highlights a complex relationship between the length of stay and psychological well-being:

* **Depression**: Longer stays are strongly correlated with higher depression scores, which might indicate worsening mental health or accumulating stress over time.
* **Social Connectedness**: Mid-length stays seem to foster better social connectedness, possibly due to better adjustment or more opportunities for social interaction.
* **Acculturative Stress**: There is a notable peak in stress related to cultural adaptation during mid-length stays, which might decrease as individuals become more accustomed to their new environment.

---