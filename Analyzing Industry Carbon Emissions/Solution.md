# <p align="center" style="margin-top: 0px;"> ⚕️ Analyzing Industry Carbon Emissions 🧠

![carbon gas](carbon_footprint.png)

---
This report contains the solution of the SQL Project 'Analyzing Industry Carbon Emissions' available on DataCamp. [Click here](https://projects.datacamp.com/projects/1593) to access the complete project.

# Introduction
When factoring heat generation required for the manufacturing and transportation of products, Greenhouse gas emissions attributable to products, from food to sneakers to appliances, make up more than 75% of global emissions. [The Carbon Catalogue 🔗](https://www.nature.com/articles/s41597-022-01178-9)

Our data, which is publicly available on [nature.com](https://www.nature.com/articles/s41597-022-01178-9), contains product carbon footprints (PCFs) for various companies. PCFs are the greenhouse gas emissions attributable to a given product, measured in CO2 (carbon dioxide equivalent).

This data is stored in a PostgreSQL database containing one table, product_emissions, which looks at PCFs by product as well as the stage of production that these emissions occurred. Here's a snapshot of what `product_emissions` contains in each column:

| field	                        | data type   |
| ------------------------------| ------------|
| id	                        | VARCHAR     |  
| year	                        | INT         |
| roduct_name                   | VARCHAR     | 
| company	                    | VARCHAR     |
| country	                    | VARCHAR     |
| industry_group                | VARCHAR     |
| weight_kg	                    | NUMERIC     |
| carbon_footprint_pcf          | NUMERIC     |
| upstream_percent_total_pcf    | VARCHAR     |
| operations_percent_total_pcf  | VARCHAR     |
| downstream_percent_total_pcf  | VARCHAR     |

The full table is provided in [this csv file](product_emissions.csv)


---
# Project Instructions: 

* Using the `product_emissions` table, find the number of unique companies and their total carbon footprint PCF for each industry group, filtering for the most recent year in the database. 
* The query should return three columns: industry_group, num_companies, and total_industry_footprint, with the last column being rounded to one decimal place. The results should be sorted by total_industry_footprint from highest to lowest values.

## Approach: 
* Since the table should return values for the most recent years, we will first  find the `MAX` of year. 

## Query: 
```sql
SELECT MAX(year)
FROM product_emissions;
```

## Output: 
| max  | 
|------|
| 2017 |

* `num_companies` will be the count of distinct companies and `total_industry_footprint` will be the sum of `carbon_footprint_pcf`, rounded to 1 decimal place. 
* The subquery to find the recent year will be included in the `WHERE` clause to filter recent years. 
* The results will be grouped by `industry_group` and ordered by `total_industry_footprint` in descending order. 

## Query:

```sql
SELECT industry_group,
	COUNT(DISTINCT company) AS num_companies,
	ROUND(SUM(carbon_footprint_pcf), 1) AS total_industry_footprint
FROM product_emissions
WHERE year IN (SELECT MAX(year) FROM product_emissions)
GROUP BY industry_group
ORDER BY total_industry_footprint DESC;
```

## Output: 
| industry_group                     | num_companies | total_industry_footprint |
|------------------------------------|---------------|--------------------------|
| Materials                          | 3             | 107129                   |
| Capital Goods                      | 2             | 94942.7                  |
| Technology Hardware & Equipment    | 4             | 21865.1                  |
| Food, Beverage & Tobacco          | 1             | 3161.5                   |
| Commercial & Professional Services| 1             | 740.6                    |
| Software & Services                | 1             | 690                      |

# Conclusion 
The Materials industry, represented by three companies, has the highest carbon footprint of 107,129 PCF, while the Software & Services industry has the lowest carbon footprint of 690 PCF.

---