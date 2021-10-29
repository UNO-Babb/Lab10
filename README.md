# Lab 10 - Visualizations with Matplotlib

For this lab we will be looking at the Covid-19 cases again and plotting them by their latitude and longitude. We will then superimpose a map underneath to create the type of graphics you have been seeing in the news and online.

## Step 1 - Import packages
We will need to use both pandas and matplotlib for this however since we don't need all of matplotlib we will only import the parts we are going to use.
```
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
```

## Step 2 - Read the data
For the data, I am again using the Johns Hopkins daily dataset. Feel free to update here: [Daily Datasets](https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_daily_reports)  
The Covid-19 data is in a subfolder called *data*.
```
covid = pd.read_csv('data/04-06-2020.csv')
```
Here we are using a pandas feature where it is easy to read all of the data from a **.csv** file. This is much more elegant than reading it in line-by-line as we did the first time we looked at this data.

Pandas also lets us break out the columns into an array numpy array of values.
```
lat = covid['Lat'].values
lon = covid['Long_'].values
confirmed = covid['Confirmed'].values
```
That was the data when we ran this project in the spring of 2020. Here is the data from last October:

```
covid = pd.read_csv('data/10-30-2020.csv')
```

## Step 3 - Plotting the Data
We can now plot the data on the latitude and longitude like this:
```
plt.scatter(lon,lat)
plt.show()
```
You will be able to see some features emerge as you plot this data. Another thing that becomes obvious is the increased granularity of the data in the United States vs. only having country data for other areas.

We could also change the scale of the data to reflect the number of cases in each region. Change the scatter function to this:
```
plt.scatter(lon, lat, s = confirmed, alpha = 0.4)
```
Okay... the number of confirmed cases has overwhelmed the system. We need to scale the information. Numpy allows us to divide by each element of an array with one division operand.
```
area = confirmed / 10000
plt.scatter(lon, lat, s = area, alpha = 0.4)
```

## Step 4 - Clean the Data
For the purposes of this lab, let's assume we are only interested in the cases within the United States. We can eliminate all of the non-US data from the .csv file or process that in our code. I have already created a new CSV that is only US cases. Change the input file:
```
covid = pd.read_csv('data/US-COVID-19.csv')
```

You'll find that the US is pushed to the corner. We want to change the scale of the graph. Play with the values until you have a suitable graph. You'll replace the **x-low** with a numeric value.

```
plt.axis([x-low, x-high, y-low, y-high])
```

## Step 5 - Overlay an Image
I found an image of North America with longitude and latitude lines that we can use.
[Source Map](https://legallandconverter.com/p45.html)
```
img = mpimg.imread('images/NorthAmericaSmall.png')
imgplot = plt.imshow(img)
```
This code should go before the *plt.show()* command.

Finally we need to adjust the image size using the extent = [left, right, bottom, top]
```
imgplot = plt.imshow(img, extent=[-190, -30, 14, 85])
```
Again, you may need to adjust the values to best fit your data on top of the map.

## Step 6 - Using Today's Data
I updated to the most recent data in the **10-28-2021.csv** file but since many areas are no longer reporting, the data is grouped by state. This mean the data is much less granular and each state is boiled down to one data-point. Fell free to play with that new data but I found it to be less interesting.

## Further Exploration
There are many other tools to plot information on a map. Here are some more resources to explore.
- https://jakevdp.github.io/PythonDataScienceHandbook/04.13-geographic-data-with-basemap.html
- https://www.geeksforgeeks.org/python-plotting-data-on-google-map-using-pygmaps-package/
- https://www.earthdatascience.org/courses/scientists-guide-to-plotting-data-in-python/plot-spatial-data/customize-vector-plots/python-customize-map-legends-geopandas/

## End of class
In Repl.it, you will find the share link to your code. That is what gets submitted to Canvas.
