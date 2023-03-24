---
layout: post
title:  "Assignment 2"
date:   2023-03-21 12:00:00 +0100
categories: jekyll update
---

Just Visualizing data is one thing, but also discovering why The data looks as it does is just as important as the data itself. In the following we are goind to explore the evolution of Vehicle Theft in San Francisco between 2003 and 2017. To do this we are goind to be using the "Police_Department_Incident_Reports__Historical_2003_to_May_2018.csv" dataset where we take the information from the "VEHICLE THEFT" category. In this paper we will visualize how vehicle thefts has changed over time, and what areas of the San Francisco area have seen the most vehicle thefts. We will also explore reasons why the evolution of thefts looks as it does.

First thing to do, in order to analyse the data, is to drag in the .csv file containting all the datapoints, along with importing all the nessessary packages:

```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import folium
from folium import plugins
from folium.plugins import HeatMap

data = pd.read_csv("../Police_Department_Incident_Reports__Historical_2003_to_May_2018.csv")
```


!!----!!
Copy text from the desktop
!!----!!



```
dataBar = data[data["Date"].str.contains("2018") == False]
dataBar = dataBar[dataBar["Category"].str.contains("VEHICLE THEFT") == True]
dataBar = dataBar.groupby(['Category', dataBar['Date'].str[-4:]]).size()
year_labels = ["2003", "2004", "2005", "2006", "2007", "2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017"]


plt.figure(figsize=(15,5))
plt.bar(year_labels, dataBar)
```


