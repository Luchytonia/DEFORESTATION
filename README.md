# DEFORESTATION PROJECT 
![DEFORESTATION PICTURE](https://github.com/Luchytonia/DEFORESTATION/assets/54556297/aebca67e-838f-4962-af45-a3ad7fe82467)

## TABLE OF CONTENT
- [Introduction](#introduction).

- [Data Wrangling](#data-wrangling)

- [Exploratory Data Analysis](#exploratory-data-analysis)

- [Data Analysis](#data-analysis)

- [Observation](#observation)

- [Recommendation](#recommendation)

- [Limitation](#limitation)

## Introduction
In this sql project, i will be working on deforestation. In this project, we will be analyzing deforestation dataset gotten from year 1990 to 2016 to derive insight, identify trands and make data driven decision on deforestation occurence across the globe.

The dataset we will be using for this project consist of 'Forest_Area.csv', 'Land_Area.csv', and 'Region.csv'. 


## Tools
SQL Server

## Data Wrangling
### Here we import our data, check for NULL values, then perfom data cleaning
```
SELECT * FROM Forest_Area
SELECT * FROM Land_Area
SELECT * FROM Region
```
![IMPORTED DATASET](https://github.com/Luchytonia/DEFORESTATION/assets/54556297/e89f69a4-6c5a-4acf-b968-4b1872bf94b1)

we check for null values
```
SELECT forest_area_sqkm FROM Forest_Area WHERE forest_area_sqkm IS NULL;
SELECT total_area_sq_mi FROM Land_Area WHERE total_area_sq_mi IS NULL;
```
we have null values so we get the average of Forest_Area and Land_Area
```
SELECT AVG(forest_area_sqkm) FROM Forest_Area
SELECT AVG(total_area_sq_mi) FROM Land_Area
```
Here we fill up the NULL values
```
UPDATE Forest_Area SET forest_area_sqkm = 391051.83952495
WHERE forest_area_sqkm IS NULL;
```
```
UPDATE Land_Area SET total_area_sq_mi = 457095.353337975
WHERE total_area_sq_mi IS NULL;
```
We have WORLD showing under country which is an outlier so we have to remove it
```
DELETE FROM REGION
WHERE REGION = 'WORLD'
```

## Exploratory Data Analysis
Now that we have a clean dataset, lets use it to answer questions to derive insight
- What are the total number of countries involved in deforestation?
- Show the income groups of countries having total area ranging from 75,000 to 150,000 square meter?
- Calculate average area in square miles for countries in the 'upper middle income region'. Compare the result with the rest of the income categories
- Determine the total forest area in square km for countries in the 'high income' group. Compare result with the rest of the income categories.
- Show countries from each region(continent) having the highest total forest areas.

## Data Analysis
To answer the first question
```
SELECT COUNT (DISTINCT COUNTRY_NAME )  AS COUNTRIES_INVOLVED_IN_DEFORSTATION FROM
(SELECT COUNTRY_NAME, FOREST_AREA_SQKM, YEAR, RANK() OVER(PARTITION BY COUNTRY_NAME ORDER BY FOREST_AREA_SQKM) RANK FROM Forest_Area) AS COUNTRIES_INVOLVED_IN_DEFORSTATION 
WHERE RANK !=1;
```
![NUMBER OF COUNTRIES INVOLVED IN DEFORESTATION](https://github.com/Luchytonia/DEFORESTATION/assets/54556297/bd8b865f-3af2-424a-a846-c2f2bf034e43)

To answer number 2 question i joined the two tabels first then 
```
SELECT * FROM LAND_AREA L
JOIN REGION R ON L.COUNTRY_NAME = R.COUNTRY_NAME
```
```
SELECT DISTINCT L.COUNTRY_NAME, R.INCOME_GROUP FROM REGION R
JOIN LAND_AREA L ON R.COUNTRY_NAME = L.COUNTRY_NAME
WHERE TOTAL_AREA_SQ_MI BETWEEN 75000 AND 150000
ORDER BY INCOME_GROUP;
```
![INCOME GROUP OF COUNTRIES FROM 75000 TO 150000](https://github.com/Luchytonia/DEFORESTATION/assets/54556297/ae48a475-cbaf-4b2e-af69-2998f95f0b88)

To answer number 3 question
```
SELECT INCOME_GROUP, AVG(TOTAL_AREA_SQ_MI) AS AVERAGE_AREA FROM REGION R
JOIN LAND_AREA L ON R.COUNTRY_NAME = L.COUNTRY_NAME
GROUP BY INCOME_GROUP
HAVING INCOME_GROUP IN ('UPPER MIDDLE INCOME','HIGH INCOME', 'LOW INCOME', 'LOWER MIDDLE INCOME')
ORDER BY AVG(TOTAL_AREA_SQ_MI)DESC;
```
![AVERAGE AREA IN SQR MILES](https://github.com/Luchytonia/DEFORESTATION/assets/54556297/abf6bf36-a4f5-45f4-8fae-52fa166125e2)

To answer number 4 question
```
SELECT INCOME_GROUP, SUM(FOREST_AREA_SQKM) AS TOTAL_FOREST_AREA FROM REGION R
JOIN FOREST_AREA F ON R.COUNTRY_NAME = F.COUNTRY_NAME
GROUP BY INCOME_GROUP
HAVING INCOME_GROUP IN ('UPPER MIDDLE INCOME','HIGH INCOME', 'LOW INCOME', 'LOWER MIDDLE INCOME')
ORDER BY SUM(FOREST_AREA_SQKM)DESC;
```
![FOREST AREA SQR KM](https://github.com/Luchytonia/DEFORESTATION/assets/54556297/8ae88c24-84d3-4e09-b2b2-60e1cdf70cd3)

To answer number 5 question
```
WITH HIGHEST_CTE AS 
(SELECT DISTINCT R.COUNTRY_NAME, REGION, F.FOREST_AREA_SQKM, DENSE_RANK() OVER(PARTITION BY REGION ORDER BY FOREST_AREA_SQKM DESC)AS RANK FROM REGION R
JOIN FOREST_AREA F ON R.COUNTRY_NAME = F.COUNTRY_NAME)
SELECT * FROM HIGHEST_CTE
WHERE RANK = 1;
```
![HIGHEST TOATAL FOREST AREA](https://github.com/Luchytonia/DEFORESTATION/assets/54556297/5bdaec6f-487d-48f2-9c1f-c064b08c0e68)

## Observation
After the data analysis, I observed that
- We have a total of 176 countries involved in deforestation
- we have 25 countries with total area range from 75,000 to 150,000. out of that 25 countries, 10 fall under High Income, 4 fall under Low Income, 5 Lower Middle Income countries, and 6 Upper Middle Income respectively
- countries in upper middle income has the highest total average area in square miles followed by high income, then lower middle income, and low income
- countries in upper middle income has highest total forest area in square km while countires in low income has the lowest total area forest.
- We have just 7 countries with high total forest area. Brazil in Latin America & Caribbean region has the highest which is 5467050 followed by Canada in North America region having 3482730

## Recommendation
- planting of trees should be encouraged in countries with high deforestation
- High forest area increases High income. countries that fall under low income should reduce deforestation so as to increase their forest area

## Limitation
Our dataset had so many null values both in land area and forest area which was filled yp with average to enable us get the ddesired result. we had outliers in region as world was written as country name. country like afghanistan was captured in the dataset as countries involved in deforestation but a proper data analysis showed that there was no change in their deforestation data from 1990 to 2016

Click [here](#C:\Users\USER\Desktop\Data Analytics class\SQL\MY PROJECT.sql) to view the full project





