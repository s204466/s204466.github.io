---
layout: post
title:  "Assignment 2"
date:   2023-03-21 12:00:00 +0100
categories: jekyll update
---

Just Visualizing data is one thing, but also discovering why The data looks as it does is just as important as the data itself. In the following we are goind to explore the evolution of Vehicle Theft in San Francisco between 2003 and 2017. To do this we are goind to be using the "Police_Department_Incident_Reports__Historical_2003_to_May_2018.csv" dataset where we take the information from the "VEHICLE THEFT" category. In this paper we will visualize how vehicle thefts has changed over time, and what areas of the San Francisco area have seen the most vehicle thefts. We will also explore reasons why the evolution of thefts looks as it does.

First thing to do, in order to analyse the data, is to drag in the .csv file containting all the datapoints, along with importing all the nessessary packages:

```python
#Import libraries
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import folium
from folium import plugins
from folium.plugins import HeatMap

#Import .csv file
data = pd.read_csv("../Police_Department_Incident_Reports__Historical_2003_to_May_2018.csv")
```


First lets take a look at how the number of reported Vehicle thefts have changed over the years from 2003 – 2017. This can be done by making a bar-chart plot with years along the x-axis, and number of reports along the y-axis


```python
#Remove 2018 since data from that year stops in may
dataBar = data[data["Date"].str.contains("2018") == False]

#Remove everything but "VEHICLE THEFT" from the dataset
dataBar = dataBar[dataBar["Category"].str.contains("VEHICLE THEFT") == True]

#Categorize date by year
dataBar = dataBar.groupby(['Category', dataBar['Date'].str[-4:]]).size()
year_labels = ["2003", "2004", "2005", "2006", "2007", "2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017"]

#Plot the bar chart with year on the x-axis, and cases on the y-axis
plt.figure(figsize=(15,5))
plt.bar(year_labels, dataBar)
plt.xlabel("Year")
plt.ylabel("Vehicle Thefts")
plt.show()
```

![Bar Chart](https://github.com/s204466/s204466.github.io/raw/04fb25aa4d67a68fc2759248762ffd699d43d38f/Files/Bar%20Chart.png)

From here we can see that there has been a significant decrease of Vehicle Thefts with a drop of roughly 10.000 cases between 2005-2006, and that the number has stayed around 5.000-7.500 ever since. It would seem safe enough to conclude that something about car safety was done between 2005 and 2006. 


It would also be interesting to see if this decrease of cases has happened across the whole city, or if it is only in some districts of the city where Vehicle Theft has decreased. This can be done by making a map over the city and then using the latitude and longitude information from the .csv file  to place a dot on each location where a Vehicle Theft was reported. In the following we are going to be looking at a map with dots for each report, in 2005 and 2006, and see where a decrease in reports have taken place.


```python
#Remove everything but "VEHICLE THEFT" from the dataset
dataMap = data[data["Category"].str.contains("VEHICLE THEFT") == True]

#isolate data from 2005
dataMap05 = dataMap[dataMap["Date"].str.contains("2005") == True]
dfMap05 = pd.DataFrame(dataMap05)

#isolate data from 2006
dataMap06 = dataMap[dataMap["Date"].str.contains("2006") == True]
dfMap06 = pd.DataFrame(dataMap06)
```


```python
#Create dual map
m = folium.plugins.DualMap(location=[37.77, -122.40], tiles='Stamen Toner', zoom_start=13)

#Display all cases in 2005 as a little blue dot
for i in range(0, len(dfMap05)):
    folium.CircleMarker(
        location=[dfMap05.iloc[i]['Y'], dfMap05.iloc[i]['X']],
        radius=1,
        color='red',
    ).add_to(m.m1)
    
    
#Display all cases in 2006 as a little blue dot
for i in range(0, len(dfMap06)):
    folium.CircleMarker(
        location=[dfMap06.iloc[i]['Y'], dfMap06.iloc[i]['X']],
        radius=1,
        color='blue',
    ).add_to(m.m2)

m
```

![Dual Map](https://github.com/s204466/s204466.github.io/raw/0729682e18625d7431abd98083da7059e73b81f0/Files/Dual%20Map.png)

Here red dots represent cases from 2005, and the blue dots represent cases from 2006.   

Here it can be seen that all over the city, the blue dots are more sparce than the red ones, which is to be expected since the number of cases has more than halved. It is particularly in the northeastern part of the city, which would make sense since that is the most populated part of the area. We can also see that there are no cases in the park areas, particularly clear in "Golden Gate Part" and in "Presidio", which can tell us that there are no cars in these areas.

Lastly we can make a bokeh interactive plot to, more in depth, look at Vehicle Theft in each district of the city pr year.

!!--Bokeh Code--!!


So, to conclude, it is clear that something has happened in regards to stopping Vehicle Theft in San Francisco. There can be multiple reasons for this. One could be due to the invention of  'engine immobilizer systems' which are devices that block the car engine if a wrong key is used to start it. More of this can be read here: https://www.nytimes.com/2014/08/12/upshot/heres-why-stealing-cars-went-out-of-fashion.html
