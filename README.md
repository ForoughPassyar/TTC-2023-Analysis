# TTC-2023-Analysis

Ask: 

Toronto Transit Commission (TTC), operated by the City of Toronto, has provided safe and convenient transportation services across the Greater Toronto Area for over 100 years. Having provided service to over 31 billion customers, the TTC is a vital part of the fabric of Toronto and allows people to get to where they need to go. They make traveling safe and easy. With the increased expense budget of 2.6 billion for the 2024 year, there is ample opportunity to target areas of improvement within the TTC. This project will examine the bus delay data from the year 2023 to target pain points that could make the operational processes more efficient, which in turn will save time and money for the TTC. 

Prepare:

I gathered data from the City of Toronto’s open data portal. It is refreshed monthly and was last refreshed on Jan 18, 2024. It was published by the Toronto Transit Commission. The data has a quality score of gold and the contact provided is opendata@ttc.ca for further information. The data is current, open source, original, and contains a daily account of bus delays for the entirety of the 2023 year. 
Process:
In terms of processing the data, I uploaded the Excel file into BigQuery. I left the zeros, duplicates, and nulls, as I wanted to visualize them first before determining if they were relevant to my analysis or not. I later deleted entries for the average delays by direction plot, which were other than N, W, E, W, or B (both east and west). I also excluded one of the zeros from the delays by vehicle number, as this was not a legitimate vehicle number.


Analyze:


--This query is retrieving the average delay of all the TTC bus delay records for 2023 from a dataset of over 56,000 records. 

```sql
SELECT AVG(min_delay) AS average_delay

FROM ‘new-data-projects.TTC_data.ttc-bus-delay-data-2023’ 
```


--This query is retrieving the unique types of incidents, which is 13.
```
SELECT

    COUNT(DISTINCT incident) AS unique_incident_count

 FROM `new-data-projects.TTC_data.ttc-bus-delay-data-2023`
```



--This code looks for the daily delay trends.

```
SELECT Date,

    AVG(min_delay) AS total_delay_time

FROM `new-data-projects.TTC_data.ttc-bus-delay-data-2023`

GROUP BY
    Date

ORDER BY
    Date
```


--This code looks at the average delay per direction.

```
SELECT
    direction,
   
 AVG (min_delay) AS average_delay

FROM `new-data-projects.TTC_data.ttc-bus-delay-data-2023`

GROUP BY
    Direction
```


--This query looks at the average delay per route.

```
SELECT
    route,

    AVG(min_delay) AS average_delay

FROM `new-data-projects.TTC_data.ttc-bus-delay-data-2023` 

GROUP BY
    Route
```



--This query looks at the vehicles with the maximum delay.

```
SELECT 
  Vehicle,

    MAX(Min_Delay) AS maximum_delay

FROM `new-data-projects.TTC_data.ttc-bus-delay-data-2023`

GROUP BY 
  Vehicle

ORDER BY
  maximum_delay DESC

LIMIT 10;
```



--The next query looks at incident count based on incident type.

```
SELECT Incident as incident_type,

    COUNT(*) as incident_count

FROM `new-data-projects.TTC_data.ttc-bus-delay-data-2023`

GROUP BY Incident;
```


Share:

<iframe src="https://public.tableau.com/app/profile/forough.passyar5180/viz/TTC2023Dashboard/TTC2023Dashboard" width="2250" height="1260"></iframe>



Act:

Based on the data I analyzed, there were a series of insights that could be used to target pain points within the operations of the TTC, which if dealt with could save time and therefore money. 

Insights:

1)	The average delay for all the buses is 20. 25 minutes. 
2)	There were 13 distinct types of incidents related to delays. 
3)	The average delay is greatest midweek on Wednesday, followed by Fridays and Thursdays. 
4)	The greatest average delay is found within the B category, referring to both east and west directions. The average delay was 96.42 minutes for this category. 
5)	The bus routes that have the greatest average delays are the 55, 337, 121, 28, 33,77, and 167. 
6)	The particular vehicles that had maximum delays were 8530, 7962, 328, 8766, 8073, 9006, 7951, 8053, and 8594. 
7)	The plot looks at various incident types. The plot shows that the greatest incident types of bus delays are due to mechanical reasons and operational reasons.
   
Recommendations:

1)	The most immediate recommendations I would make to the TTC to reduce costs would be to check for mechanical issues with the particular vehicles numbered 8530, 7962, 328, 8766, 8073, 9006, 7951, 8053, and 8594. Given that most of the incident delays are due to mechanical issues. This would likely reduce delays.
   
2)	The bus routes that had the greatest average delays were 55, 37, 121, 28, 33, 77, and 167 and they were traveling in all different directions. I speculate that since the greatest delays were in the east/ west direction, the delays may be due to east/west traffic. Furthermore, since Wednesday, Thursday, and Friday have the most delays, I can also reasonably conclude these are probably traffic-related. The solution might be infrastructural issues beyond the scope and power of the TTC, nonetheless, I think this analysis is useful for illuminating pain points which in turn create inefficiency within the TTC. 


Citations:

https://open.toronto.ca/dataset/ttc-bus-delay-data/ 

https://www.cp24.com/news/ttc-proposes-2024-budget-no-fare-increases-included-1.6695571#:~:text=The%20TTC%20will%20freeze%20fares,a%20special%20meeting%20on%20Wednesday 
