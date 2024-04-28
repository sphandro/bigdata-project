# CIS 4560 Bigdata Project

| Group 2              |                      |                      |                      |                      |
|:---------------------|:---------------------|:---------------------|:---------------------|:---------------------|
| Ethan Cheng          | John Haavardsen      | Nemesio Merales      | Alex Monita-Guzman   | Zichen Xue           |

## Abstract
This project will present an analysis of traffic congestion patterns in the Contiguous United States before and after the COVID-19 shutdowns, focusing on data manipulation and analysis techniques. The study will leverage a large 12GB dataset spanning from February 2016 to September 2022, comprising over 33 million recorded congestion events across the United States. The analysis involves temporal segmentation of the dataset into pre-COVID and post-COVID periods, followed by the calculation and comparison of congestion metrics. The project will utilize Hadoop and Hive for data handling and cleaning processes and Excel and Power BI for data analysis and visualization. The results from this study will offer insights into transportation planning and policy-making efforts surrounding events like large-scale shutdowns. 

## Dataset Source
US Traffic Congestions (2016-2022) 12.88 GB 

https://www.kaggle.com/datasets/sobhanmoosavi/us-traffic-congestions-2016-2022

## Metadata
| Data Point | Description |
|:-------|:---------|
| **ID** | Event ID |
| **Severity** | Severity of event, as reported by the main data provider, is rated on a scale from 0 to 4, with 0 being the least severe. |
| **Start_Lat** | Approximate latitude coordinate of the congestion event. |
| **Start_Lng** | Approximate longitude coordinate of the congestion event. |
| **StartTime** | Approximate start time of the congestion event, given in the local timezone. |
| **EndTime** | Approximate end time of the congestion event, given in the local timezone.  |
| **Distance (mi)** | The length of the road extent affected by the congestion event, measured in miles. |
| **DelayFromTypicalTraffic (mins)** | Delay compared to typical traffic flow (in minutes) due to the congestion event, as reported by the provider. |
| **DelayFromFreeFlowSpeed  (mins)** | Delay compared to free traffic flow (in minutes) due to the congestion event, as reported by the provider. |
| **Congestion_Speed** |  The categorically ranked speed of traffic impacted by the congestion, as reported by the provider. |
| Description | Text description |
| Street |  |
| City |  |
| County |  |
| State |  |
| Country |  |
| ZipCode |  |
| LocalTimeZone |  |
| WeatherStation_AirportCode |  |
| WeatherTimeStamp |  |
| Temperature |  Fahrenheit |
| WindChill |  Fahrenheit |
| Humidity |  % |
| Pressure | inches |
| Visibility | miles |
| WindDir |  |
| WindSpeed | miles per hour  |
| Precipitation | inches |
| Weather_Event |  |
| Weather_Conditions |  |
|        |          |

# Setup
## Prep dataset
Navigate into dataset directory and uncompress the split files

```
cd dataset/
tar -xzf split-aa.tar.gz
tar -xzf split-ab.tar.gz
```

Merge parts into one csv file

```
cat split-aa split-ab > us_congestion.csv
```

## Create HDFS directory
```
hdfs dfs mkdir /tmp/data/congestion
hdfs dfs -put us_congestion.csv /tmp/data/congestion
hdfs dfs -ls /tmp/data/congestion
```

## Create Hive Table
```
use [username];

DROP TABLE IF EXISTS congestion;

CREATE EXTERNAL TABLE IF NOT EXISTS us_congestion(
   ID STRING,
   Severity TINYINT,
   Start_Lat DOUBLE,
   Start_Lng DOUBLE,
   StartTime TIMESTAMP,
   EndTime TIMESTAMP,
   Distance DOUBLE,
   DelayFromTypicalTraffic TINYINT,
   DelayFromFreeFlowSpeed TINYINT,
   Congestion_Speed STRING,
   Description STRING,
   Street STRING,
   City STRING,
   County STRING,
   State STRING,
   Country STRING,
   ZipCode INT,
   LocalTimeZone STRING,
   WeatherStation_AirportCode STRING,
   WeatherTimeStamp TIMESTAMP,
   Temperature TINYINT,
   WindChill TINYINT,
   Humidity TINYINT,
   Pressure DOUBLE,
   Visibility TINYINT, 
   WindDir STRING,
   WindSpeed TINYINT,
   Precipitation TINYINT,
   Weather_Event STRING,
   Weather_Conditions STRING )
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ','
STORED AS TEXTFILE LOCATION '/user/amonita/tmp/data/congestion'
TBLPROPERTIES ('skip.header.line.count'='1');
```


# Process
1. Temporal Segmentation: Divide the dataset into two segments: pre-COVID period and post-COVID period. The pre-COVID period could be defined as the time period before the implementation of widespread COVID-19 shutdowns and restrictions, while the post-COVID period could be defined as the time period after these measures were implemented. 

2. Congestion Metrics Calculation: Calculate relevant congestion metrics for each segment, such as: 

   - Average congestion severity: Calculate the average severity of congestion events (e.g., 	severity column) for each segment. 

   - Total congestion duration: Calculate the total duration of congestion events (e.g., using StartTime and EndTime columns) for each segment. 

   - Congestion frequency: Count the number of congestion events for each segment. 

   - Delay metrics: Calculate average delay times (e.g., DelayFromTypicalTraffic and DelayFromFreeFlowSpeed columns) for each segment. 

3. Comparison Analysis: Compare the congestion metrics between the pre-COVID and post-COVID periods to identify any significant differences or trends: 

   - Severity comparison: Determine if there are differences in the severity of congestion events before and after the COVID-19 shutdown. 

   - Duration comparison: Analyze if there are changes in the total duration or frequency of congestion events between the two periods. 

   - Delay comparison: Compare average delay times before and after the COVID-19 shutdown to assess changes in travel times and congestion impacts. 

4. Visualization: Visualize the congestion metrics using charts, graphs, or 3D maps to effectively communicate the findings of our analysis. We will create time series plots showing congestion trends over time, comparing congestion metrics between the pre-COVID and post-COVID periods, or spatial maps illustrating changes in congestion patterns across different regions. 

5. Statistical Analysis: Conduct statistical tests, such as t-tests to determine if the observed differences in congestion metrics between the two periods are statistically significant.

By analyzing the amount of congestion before and after the COVID-19 shutdown using this approach, we can gain insights into how the pandemic and associated measures impacted traffic patterns, congestion levels, and travel behaviors. This analysis could provide valuable information for transportation planning, policy-making, and infrastructure management efforts. 