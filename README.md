![Factories creating emissions](pollution.jpg)
Photo by Maxim Tolchinskiy on Unsplash

_Greenhouse gas emissions attributable to products&mdash;from food to sneakers to appliances&mdash;make up more than 75% of global emissions._ -[The Carbon Catalogue](https://www.nature.com/articles/s41597-022-01178-9)

# Examining Product Carbon Footprints

Our data, which is publicly available on [nature.com](https://www.nature.com/articles/s41597-022-01178-9), contains product carbon footprints (PCFs) for various companies. PCFs are the greenhouse gas emissions attributable to a given product, measured in CO<sub>2</sub> (carbon dioxide equivalent).

This data is stored in a PostgreSQL database containing one table, `product_emissions`, which looks at PCFs by product as well as the stage of production these emissions occurred in. Here's a snapshot of what `product_emissions` contains in each column:

### `product_emissions` Table

| field                              | data_type |
|------------------------------------|-----------|
| id                                 | VARCHAR   |
| year                               | INT       |
| product_name                       | VARCHAR   |
| company                            | VARCHAR   |
| country                            | VARCHAR   |
| industry_group                     | VARCHAR   |
| weight_kg                          | NUMERIC   |
| carbon_footprint_pcf               | NUMERIC   |
| upstream_percent_total_pcf         | VARCHAR   |
| operations_percent_total_pcf       | VARCHAR   |
| downstream_percent_total_pcf       | VARCHAR   |

## Analysis: Carbon Footprint by Industry in 2017

industry_group                   | count_industry | total_industry_footprint
----------------------------------|----------------|-------------------------
Materials                        | 11             | 107,129.0
Capital Goods                    | 4              | 94,942.7
Technology Hardware & Equipment  | 22             | 21,865.1
Food, Beverage & Tobacco         | 22             | 3,161.5
Commercial & Professional Services | 2              | 740.6
Software & Services              | 1              | 690.0

Conclusion
Based on the analysis of the product carbon footprints in 2017, the industries with the highest total carbon footprints are:

Materials with a total footprint of 107,129.0
Capital Goods with a total footprint of 94,942.7
Technology Hardware & Equipment with a total footprint of 21,865.1
On the other hand, the industries with relatively lower total carbon footprints are:

Food, Beverage & Tobacco with a total footprint of 3,161.5
Commercial & Professional Services with a total footprint of 740.6
Software & Services with a total footprint of 690.0
These findings provide insights into the carbon emissions associated with different industries and can be valuable for identifying areas of focus for reducing greenhouse gas emissions.

To examine the carbon footprint of each industry in the dataset, the following SQL query was used:

```sql
SELECT industry_group,
    COUNT(company) AS count_industry,
    ROUND(SUM(carbon_footprint_pcf), 1) AS total_industry_footprint
FROM product_emissions
GROUP BY industry_group, year
HAVING year = 2017
ORDER BY total_industry_footprint DESC; 



