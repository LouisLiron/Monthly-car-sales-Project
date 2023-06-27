# Monthly-car-sales-Project

## Abstract
An exciting data analysis project awaits! I've have obtained a dataset from an anonymous company, consisting of 108 data points with two variables: "Months" and "Sales", which makes it very suitable for a predictive model. My mission is to dive deep into this dataset, unravel its mysteries, and develop predictive models that will reveal what the future holds.

## Variables
- Months
- Sales

## Objective
1. Understand the data
2. Build a predictive model that help to unravel what the future holds.

## Analysis
1. Understand the data

We'll look at insights from the Exploratory Analysis to help us.

```python
#Obtaining a descriptive statistics of the dataset
round(data.describe(), 1)
```
**Results**
    ||Sales|
|-----|-----|
|count	|108.0|
|mean|	14595.1|
|std| 4525.2|
|min	|5568.0|
|25%	|11391.2|
|50%	|14076.0|
|75%	|17595.8|
|max|26099.0|

The summary statistics provided can provide some insights about the dataset. Here's what each statistic tells us:

- Count: The count indicates the number of data points available for the "Sales" column, which in this case is 108. It tells us that there are 108 records with valid sales values.

- Mean: The mean represents the average value of the "Sales" column, which is calculated as 14595.1. It gives us an idea of the central tendency of the sales values in the dataset.

- Standard Deviation (Std): The standard deviation measures the dispersion or variability of the "Sales" values around the mean. In this case, the standard deviation is 4525.2. A higher standard deviation suggests greater variability in the sales data points.

- Min: The minimum value of the "Sales" column is 5568.0. It represents the lowest recorded sales value in the dataset.

- 25%: The 25th percentile, also known as the first quartile, is 11391.2. This means that 25% of the sales values in the dataset are below this value.

- 50%: The 50th percentile, also known as the median, is 14076.0. It represents the middle value of the sorted sales data. 50% of the sales values are below this median value.

- 75%: The 75th percentile, also known as the third quartile, is 17595.8. This means that 75% of the sales values in the dataset are below this value.

- Max: The maximum value of the "Sales" column is 26099.0. It represents the highest recorded sales value in the dataset.

From these summary statistics, we can gather information about the central tendency, variability, and distribution of the sales data. It helps us understand the range of sales values, identify outliers, and assess the overall distribution of the dataset.

```python
#having a quick visualisation to know how it looks on a graph
data.plot()
plt.grid()
```
**Results**
![image](https://github.com/LouisLiron/Monthly-car-sales-Project/assets/124049051/0939aeec-5568-4d0d-816a-ed9b2bb064cf)

Based on the information provided, the image indicates a positive trend in sales since the inception of the company. The presence of an upward trend in the chart suggests consistent growth in revenue over time.

```python
#A histigram helps to understand the distribution of the data
plt.hist(data, bins=20, density=True)
plt.xlabel('Values')
plt.ylabel('Frequency')
plt.title('Histogram')
plt.grid()
```
**Results**
![image](https://github.com/LouisLiron/Monthly-car-sales-Project/assets/124049051/34831cf2-c6f0-4654-b2c3-e8dbeb07281c)

The histogram presented above represents the values of cars and their corresponding frequencies within the dataset. The histogram reveals that values slightly below 15,000 have the highest occurrence or frequency, indicating that a significant number of cars fall within this range. On the other hand, values around 25,000 have the lowest frequency, suggesting that fewer cars are observed in this range. The overall shape of the histogram resembles a bell-shaped distribution, although not perfectly symmetric.

```python
#plotting a boxplot to better understand the distribution
plt.boxplot(data)
plt.xlabel('Data')
plt.ylabel('Values')
plt.title('Boxplot')
plt.grid()
```
**Results**
![image](https://github.com/LouisLiron/Monthly-car-sales-Project/assets/124049051/b42aa4e5-e287-4b10-9887-1aeb6fe8f648)

The boxplot provides valuable insights into the distribution of the data. The central line within the box represents the median value, which is the value that divides the dataset into two equal halves. In this case, the median is calculated to be 14076.0.

The box itself represents the interquartile range (IQR), which encompasses the middle 50% of the data. It is defined by the 1st quartile (25th percentile) and the 3rd quartile (75th percentile). The majority of the data points are concentrated within this range, indicating where the bulk of the data is located.

The whiskers in the boxplot indicate the minimum and maximum values of the dataset, excluding any outliers. The distance between the whiskers and the box represents the data points that fall outside the upper or lower quartile. These points are considered potential outliers or extreme values.

Overall, the boxplot provides a visual summary of the dataset, highlighting the central tendency, spread, and presence of any outliers.

Let's take a look at some SQL syntax;
~~~SQL
--viewing the date range 
SELECT 
    CAST(MAX(Month)AS date) AS Max_date,
    CAST(MIN(Month)AS date) AS Min_date, 
    CONCAT(CAST(MAX(Month)-MIN(Month)AS INT), ' ', 'days') AS Date_range
FROM
    Monthly_car_sales
~~~
**Results**
|Max_date|Min_date|Date_range|
|-----|------|-----|
|1968-12-01|1960-01-01|3257 days|

The dataset in question spans a duration of 3257 days, which is equivalent to a total of 9 years. This indicates that the data encompasses a period of 9 years, from the beginning to the end.

~~~SQL
SELECT DISTINCT
    sub.Years AS Years, 
    SUM(sub.Sales) AS Sales, 
    COALESCE(SUM(sub.Sales) - LAG(SUM(sub.Sales)) OVER (ORDER BY sub.Years), 0) AS YoY_change,
    CONCAT(ROUND(COALESCE(SUM(sub.Sales) - LAG(SUM(sub.Sales)) OVER (ORDER BY sub.Years), 0) / SUM(sub.Sales), 2) * 100, '%') AS YoY_Pct_change

FROM (
SELECT DISTINCT
	YEAR(Month) AS Years, 
	SUM(Sales) AS Sales
FROM Monthly_car_sales
GROUP BY Month
) AS sub
GROUP BY sub.Years
~~~

|Years	|Sales	|YoY_change	|YoY_Pct_change|
|-----|------|-----|-----|
|1960|122240|0	|0%
|1961|130297|8057|	6%|
|1962|151960|21663|	14%|
|1963|165923|13963|	8%|
|1964|182053|16130|	9%|
|1965|205338|23285|	11%|
|1966|200747|-4591|	-2%|
|1967|198976|-1771|	-1%|
|1968|218738|19762|	9%|

According to the table, we can observe a notable upward trend in sales since the company's establishment. However, during the years 1966 and 1967, there was an unexplained decline in sales, with a decrease of 2% and 1% respectively, resulting in a total decrease of 3%. However, the following year witnessed a swift recovery, with sales revenue experiencing a significant 9% increase.


2. Build a predictive model that hepl to unravel what the future holds.
When faced with the decision of selecting a predictive model, I was uncertain between ARIMA and Holt-Winters models. To resolve this, I decided to create both models and compare their performance to determine which one would yield better results. By evaluating their respective outputs, I can determine which model is more effective in forecasting the desired outcome.

## ARIMA Model
Implementing the ARIMA model demanded significant planning and effort, but we persevered and successfully completed the task as anticipated.




