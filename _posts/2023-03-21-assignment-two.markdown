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


!!----!!
Copy Bar text from the desktop
!!----!!



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


!!----!!
Copy Map text from the desktop
!!----!!



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
#Load in the map
mapSF = folium.Map(location=[37.77, -122.40],
                        tiles = "Stamen Toner",
                    zoom_start = 13) 

#Display all cases in 2006 as a little blue dot
for i in range(0, len(dfMap06)):
    folium.CircleMarker(
        location=[dfMap06.iloc[i]['Y'], dfMap06.iloc[i]['X']],
        radius=1,
        color='blue',
    ).add_to(mapSF)

#Display all cases in 2005 as a little red dot
for i in range(0, len(dfMap05)):
    folium.CircleMarker(
        location=[dfMap05.iloc[i]['Y'], dfMap05.iloc[i]['X']],
        radius=1,
        color='red',
    ).add_to(mapSF)

mapSF
```

![Map](https://github.com/s204466/s204466.github.io/raw/3d26f4f1df67929499c86dbcb767904324f40cef/Files/Map.png)

