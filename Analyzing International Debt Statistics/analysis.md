# <p align="center" style="margin-top: 0px;"> 💸 Analyze International Debt Statistics 📊

![dollar bill](dollar%20bill.png)

---
This report contains the solution of the SQL Project 'Analyze International Debt Statics' available on DataCamp. To access the complete project click on this [link](https://app.datacamp.com/learn/projects/1906).

# Introduction
Humans not only take debts to manage necessities. A country may also take debt to manage its economy. For example, infrastructure spending is one costly ingredient required for a country's citizens to lead comfortable lives. The World Bank is the organization that provides debt to countries.

In this project, you are going to analyze international debt data collected by The World Bank. The dataset contains information about the amount of debt (in USD) owed by developing countries across several categories. You are going to find the answers to the following questions:

* What is the number of distinct countries present in the database?
* What country has the highest amount of debt?
* What country has the lowest amount of repayments?

Access has been granted to the `international_debt` table, which is as follows: 

| Column          | Definition                                       | Data Type |
|-----------------|--------------------------------------------------|-----------|
| country_name    | Name of the country                              | varchar   |
| country_code    | Code representing the country                     | varchar   |
| indicator_name  | Description of the debt indicator                 | varchar   |
| indicator_code  | Code representing the debt indicator              | varchar   |
| debt            | Value of the debt indicator for the given country | float     |

The full table is provided in [this csv file](international_debt.csv)

First, we will take a look at the data in hand by selecting all the columns. Limiting the data to 10 rows to keep the output clean.

```sql
SELECT *
FROM international_debt
LIMIT 10
```

Output:
|   | country_name  | country_code | indicator_name                                                 | indicator_code   | debt         |
|---|---------------|--------------|----------------------------------------------------------------|------------------|--------------|
| 0 | Afghanistan   | AFG          | Disbursements on external debt, long-term (DIS, current US$)  | DT.DIS.DLXF.CD  | 72894453.7   |
| 1 | Afghanistan   | AFG          | Interest payments on external debt, long-term (INT, current US$) | DT.INT.DLXF.CD  | 53239440.1   |
| 2 | Afghanistan   | AFG          | PPG, bilateral (AMT, current US$)                             | DT.AMT.BLAT.CD  | 61739336.9   |
| 3 | Afghanistan   | AFG          | PPG, bilateral (DIS, current US$)                             | DT.DIS.BLAT.CD  | 49114729.4   |
| 4 | Afghanistan   | AFG          | PPG, bilateral (INT, current US$)                             | DT.INT.BLAT.CD  | 39903620.1   |
| 5 | Afghanistan   | AFG          | PPG, multilateral (AMT, current US$)                          | DT.AMT.MLAT.CD  | 39107845     |
| 6 | Afghanistan   | AFG          | PPG, multilateral (DIS, current US$)                          | DT.DIS.MLAT.CD  | 23779724.3   |
| 7 | Afghanistan   | AFG          | PPG, multilateral (INT, current US$)                          | DT.INT.MLAT.CD  | 13335820     |
| 8 | Afghanistan   | AFG          | PPG, official creditors (AMT, current US$)                    | DT.AMT.OFFT.CD  | 100847181.9  |
| 9 | Afghanistan   | AFG          | PPG, official creditors (DIS, current US$)                    | DT.DIS.OFFT.CD  | 72894453.7   |

---
# Questions

## 1. What is the number of distinct countries present in the database?
* The output should be a single row and column aliased as `total_distinct_countries`.

**Approach:**
This is pretty straightforward. Use the `count` function along with the `distinct` keyword for `country_name`.

```sql
SELECT COUNT(DISTINCT country_code) AS total_distinct_countries
FROM public.international_debt
```

Output: 
| total_distinct_countries |
|--------------------------|
|           124            |

## 2. What country has the highest amount of debt?
* The output should contain two columns: `country_name` and `total_debt` and one row.

**Approach:**
Since a country has multiple entries for debts for different debt indicators, the `total_debt` will be the sum of all debts. 

### Query: 

```sql
SELECT 
	country_name, 
	SUM(debt) AS total_debt
FROM public.international_debt 
GROUP BY country_name 
ORDER BY SUM(debt) DESC 
LIMIT 1;
```

### Output: 
| country_name | total_debt       |
|--------------|------------------|
| China        | 285793494734.2   |


## 3. What country has the lowest amount of principal repayments (indicated by the "DT.AMT.DLXF.CD" indicator code)?
* The output table should contain three columns: `country_name`, `indicator_name`, and `lowest_repayment` and one row.

**Approach:** 
* The `SUM` function will be used to calculate the total debt, followed by the `WHERE` clause to filter the results based on the given `indicator_code`.
* The results will be grouped by `indicator_name` and `country_name`, with the data sorted in ascending order of debt to obtain results alphabetically from A to Z.
* Limiting the results to 1 as only the lowest amount is required.

### Query: 
```sql
SELECT 
	country_name, 
	indicator_name, 
	SUM(debt) AS lowest_repayment
FROM public.international_debt
WHERE indicator_code = 'DT.AMT.DLXF.CD'
GROUP BY indicator_name, country_name
ORDER BY SUM(debt) ASC
LIMIT 1
```

### Output: 
| country_name | indicator_name                                                 | lowest_repayment |
|--------------|----------------------------------------------------------------|------------------|
| Timor-Leste  | Principal repayments on external debt, long-term (AMT, current US$) | 825000      |

