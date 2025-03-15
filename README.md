# Air Quality Index Analysis

## Overview
I have conducted an Air Quality Index (AQI) Analysis to provide a numerical representation of air quality. This analysis is essential for public health and environmental management.

## Steps Followed in AQI Analysis

1. **Data Collection**: I gathered air quality data from various sources, such as government monitoring stations, sensors, and satellite imagery.
2. **Data Cleaning & Preprocessing**: I cleaned and preprocessed the collected data to ensure accuracy.
3. **AQI Calculation**: I computed the Air Quality Index using standardized formulas provided by environmental agencies.
4. **Data Visualization**: I created visualizations such as line charts and heatmaps to represent AQI over time and across geographical regions.
5. **Comparative Analysis**: I compared the AQI metrics of the location with recommended air quality standards.

## Air Quality Index Analysis using Python

### Importing Libraries & Dataset 
Link to Dataset used in this Analysis. [Dataset](https://statso.io/air-quality-index-analysis-case-study/)
```python
import pandas as pd
import plotly.express as px
import plotly.io as pio
import plotly.graph_objects as go

pio.templates.default = "plotly_white"

data = pd.read_csv("delhiaqi.csv")
print(data.head())
```

### Sample Data
```
date       co     no    no2    o3    so2   pm2_5    pm10    nh3  
0  2023-01-01 00:00:00  1655.58   1.66  39.41  5.90  17.88  169.29  194.64   5.83  
1  2023-01-01 01:00:00  1869.20   6.82  42.16  1.99  22.17  182.84  211.08   7.66  
...
```

### Converting Date Column to Datetime Format
```python
data['date'] = pd.to_datetime(data['date'])
```

### Descriptive Statistics
```python
print(data.describe())
```
```
         co          no         no2          o3         so2   pm2_5   pm10   nh3  
count    561.000000  ...  
mean    3814.942210  ...  
std     3227.744681  ...  
...
```

### Visualizing Pollutant Intensities Over Time
![Pollutant Intensity Time Series](https://github.com/Sourabh1710/Air-Quality-Index-Analysis/blob/main/images/Time%20Series%20Analysis%20of%20Air%20Pollutants%20in%20Delhi.png)

## Calculating Air Quality Index

### AQI Calculation Logic
```python
# Define AQI breakpoints and corresponding AQI values
aqi_breakpoints = [
    (0, 12.0, 50), (12.1, 35.4, 100), (35.5, 55.4, 150),
    (55.5, 150.4, 200), (150.5, 250.4, 300), (250.5, 350.4, 400),
    (350.5, 500.4, 500)
]

def calculate_aqi(pollutant_name, concentration):
    for low, high, aqi in aqi_breakpoints:
        if low <= concentration <= high:
            return aqi
    return None

def calculate_overall_aqi(row):
    aqi_values = []
    pollutants = ['co', 'no', 'no2', 'o3', 'so2', 'pm2_5', 'pm10', 'nh3']
    for pollutant in pollutants:
        aqi = calculate_aqi(pollutant, row[pollutant])
        if aqi is not None:
            aqi_values.append(aqi)
    return max(aqi_values)

data['AQI'] = data.apply(calculate_overall_aqi, axis=1)
```

### Categorizing AQI
```python
# Define AQI categories
aqi_categories = [
    (0, 50, 'Good'), (51, 100, 'Moderate'), (101, 150, 'Unhealthy for Sensitive Groups'),
    (151, 200, 'Unhealthy'), (201, 300, 'Very Unhealthy'), (301, 500, 'Hazardous')
]

def categorize_aqi(aqi_value):
    for low, high, category in aqi_categories:
        if low <= aqi_value <= high:
            return category
    return None

data['AQI Category'] = data['AQI'].apply(categorize_aqi)
print(data.head())
```

### AQI Calculation Output
```
date       co     no    no2    o3    so2   pm2_5    pm10    nh3  AQI    AQI Category  
0 2023-01-01 00:00:00  1655.58   1.66  39.41  5.90  17.88  169.29  194.64   5.83  300  Very Unhealthy  
1 2023-01-01 01:00:00  1869.20   6.82  42.16  1.99  22.17  182.84  211.08   7.66  300  Very Unhealthy  
...
```

### Analyzing AQI of Delhi
![AQI Trends in January](https://github.com/Sourabh1710/Air-Quality-Index-Analysis/blob/main/images/AQI%20of%20Delhi%20in%20January.png)

### AQI Category Distribution
![AQI Category Distribution](https://github.com/Sourabh1710/Air-Quality-Index-Analysis/blob/main/images/AQI%20Category%20Distribution%20Over%20Time.png)

### Pollutant Distribution
![Pollutant Distribution](https://github.com/Sourabh1710/Air-Quality-Index-Analysis/blob/main/images/Pollutant%20Concentrations%20in%20Delhi.png)

### Correlation Between Pollutants
![Correlation Matrix](https://github.com/Sourabh1710/Air-Quality-Index-Analysis/blob/main/images/Correlation%20Between%20Pollutants.png)

### Hourly Average AQI Trends
![Hourly AQI Trends](https://github.com/Sourabh1710/Air-Quality-Index-Analysis/blob/main/images/Hourly%20Average%20AQI%20Trends%20in%20Delhi%20(Jan%202023).png)

### Average AQI by Day of the Week
![Weekly AQI Trends](https://github.com/Sourabh1710/Air-Quality-Index-Analysis/blob/main/images/Average%20AQI%20by%20Day%20of%20the%20Week.png)

## Summary
Air Quality Index (AQI) analysis is a crucial aspect of environmental data science that involves monitoring and analyzing air quality in a specific location. I have implemented AQI computation, categorized AQI levels, and created visualizations to study air pollution trends in Delhi.

## Author
Sourabh Sonker <br>
Data Scientist
