
## Pyber Ride Sharing Data Analysis

Analysis:

-	Fare cost correlates to the amount of drivers available in a city. More drivers available means lower fare cost; while lower amount of drivers usually means higher fare costs.


-	A large amount of rides are taken in urban areas – which is heavily influence by large amount of available drivers in urban areas (77% of all drivers come from urban areas). A conclusion can be made that more drivers allows for more rides.


-	The inverse of this previous point is also true – less drivers means less rides. Out of the three area types, rural area ranks the lowest in terms of both driver and ride total. This does not necessarily mean that there is a lack of a market share in rural areas, but rather that there could a demand that is not being met due to lack of drivers. 
      - There is clearly a demand for rideshare since the market is willing to pay a high fare cost in rural areas. 




```python
# Importing libs
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import os
import seaborn as sns

from matplotlib.legend_handler import HandlerLine2D
```


```python
# CSV reading
csv_data_city = os.path.join('city_data.csv')
csv_data_ride = os.path.join('ride_data.csv')

city_data_df = pd.read_csv(csv_data_city)
ride_data_df = pd.read_csv(csv_data_ride)


# Output City header
city_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Output Rider header
ride_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Merging all data into one df
city_ride_df = pd.merge(ride_data_df, city_data_df, on = "city", how="outer")
city_ride_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sarabury</td>
      <td>2016-07-23 07:42:44</td>
      <td>21.76</td>
      <td>7546681945283</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarabury</td>
      <td>2016-04-02 04:32:25</td>
      <td>38.03</td>
      <td>4932495851866</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarabury</td>
      <td>2016-06-23 05:03:41</td>
      <td>26.82</td>
      <td>6711035373406</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sarabury</td>
      <td>2016-09-30 12:48:34</td>
      <td>30.30</td>
      <td>6388737278232</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_data_unique = city_ride_df.loc[:, ["city", "driver_count", "type"]]

city_data_unique = city_data_unique.drop_duplicates(['city'])

city_data_unique.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>27</th>
      <td>South Roy</td>
      <td>35</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Wiseborough</td>
      <td>55</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Spencertown</td>
      <td>68</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# number of of rides per city
num_rides = ride_data_df.groupby(["city"])["ride_id"].count()

num_rides_df = pd.DataFrame({"num_rides": num_rides})


num_rides_df = num_rides_df.reset_index()

num_rides_df.head()
# num_rides_df = num_rides_df.drop_duplicates(subset = 'city')
# num_rides_df.loc[num_rides_df['city'] == 'Port James'] #sanity check
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>num_rides</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
# add column for mean fares after grouping df by city

mean_fare = city_ride_df.groupby(["city"])["fare"].mean()


#Create a dataframe to hold the city, driver count, and mean fare
#summary_table = city_data_unique
#summary_table["mean_fare"] = mean_fare


mean_fare_df = pd.DataFrame({"mean_fare": mean_fare})
mean_fare_df = mean_fare_df.reset_index()
mean_fare_df.head()

# mean_fare_df.loc[mean_fare_df['city'] == 'Port James']
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>mean_fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>23.928710</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>20.609615</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>37.315556</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>23.625000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>21.981579</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Combining into ultimate dataframe
merged_df = pd.merge(city_data_unique, mean_fare_df, on= "city", how="outer")
merged_df = pd.merge(merged_df, num_rides_df, on= "city", how="outer")
merged_df.head()

# Dataframes for each area type
urban_df = merged_df.loc[merged_df['type'] == 'Urban']
suburban_df = merged_df.loc[merged_df['type'] == 'Suburban']
rural_df = merged_df.loc[merged_df['type'] == 'Rural']
```


```python
# Setting something in 
sns.set()

# Labels
plt.title("Pyber Ride Sharing Data (20XX)")
plt.xlabel("Total Nmber of Rides (Per City)")
plt.ylabel("Average Fare ($)")

# Setting X Y limits (based on +5/-5 of min/max of fare)
plt.ylim( min(merged_df["mean_fare"]) - 5, max(merged_df["mean_fare"]) + 5)

# Setting plot based on rides/mean and color
lightcoral = plt.scatter(urban_df["num_rides"], urban_df["mean_fare"], s=urban_df["driver_count"], c='lightcoral', edgecolor = 'black')
lightskyblue = plt.scatter(suburban_df["num_rides"], suburban_df["mean_fare"], s=suburban_df["driver_count"], c='lightskyblue', edgecolor = 'black')
gold = plt.scatter(rural_df["num_rides"], rural_df["mean_fare"], s=rural_df["driver_count"], c='gold',edgecolor = 'black')

# plotting it up
plt.legend([lightcoral, lightskyblue, gold], ["Urban", "Suburban", "Rural"])

# render scatterplot
plt.show()
```


![png](output_9_0.png)



```python
# In addition, you will be expected to produce the following three pie charts:
# % of Total Fares by City Type
# % of Total Rides by City Type
# % of Total Drivers by City Type
```


```python
### Pie Chart - % of Total Fares by City Type
total_fares = city_ride_df["fare"].sum()

total_fares_type = city_ride_df.groupby(["type"]).sum()
#total_fares_type

total_fares_type["pct_total_fare"] = (total_fares_type["fare"]/total_fares) * 100

# create pie chart
col_list = pd.DataFrame({"type": total_fares_type.index.values})
col_map = col_list["type"].map({"Urban": "lightcoral", "Suburban": "lightskyblue", "Rural": "gold"})
explode = [0,0,.1] # PAC MAN NOM NOM NOM NOM

plt.pie(total_fares_type["pct_total_fare"], labels=total_fares_type.index, colors=col_map,
       autopct="%1.1f%%", shadow=True, startangle=140, explode=explode)

#shadow, startangle, and explosion

#print(total_fares_type)
plt.title("% of Total Fare by City Type")
plt.show()


# EVIL PAC MAN NOM NOM NOM NOM
```


![png](output_11_0.png)



```python
### Pie Chart - % Total Rides by City Type
total_rides = city_ride_df["fare"].count()

total_rides_type = city_ride_df.groupby(["type"]).count()

total_rides_type["pct_total_rides"] = (total_rides_type["fare"]/total_rides) * 100

#total_rides_type

# create pie chart
explode = [0,0,.1]
col_list = pd.DataFrame({"type": total_rides_type.index.values})
col_map = col_list["type"].map({"Urban": "lightcoral", "Suburban": "lightskyblue", "Rural": "gold"})
plt.pie(total_rides_type["pct_total_rides"], labels=total_rides_type.index, colors=col_map,
       autopct="%1.1f%%", shadow=True, startangle=140, explode=explode)

#print(total_fares_type)
plt.title("% of Total Rides by City Type")
plt.show()
```


![png](output_12_0.png)



```python
### Pie Chart - % Total Driver by City Type
total_drivers = city_data_df["driver_count"].sum()
total_drivers_type = city_data_df.groupby(["type"]).sum()

total_drivers_type["pct_total_drivers"] = (total_drivers_type["driver_count"]/total_drivers) * 100

#total_drivers_type

# create pie chart
explode = [0,0,.1]
col_list = pd.DataFrame({"type": total_drivers_type.index.values})
col_map = col_list["type"].map({"Urban": "lightcoral", "Suburban": "lightskyblue", "Rural": "gold"})
plt.pie(total_drivers_type["pct_total_drivers"], labels=total_drivers_type.index, colors=col_map,
       autopct="%1.1f%%", shadow=True, startangle=140, explode=explode)

#print(total_fares_type)
plt.title("% of Total Drivers by City Type")
plt.show()
```


![png](output_13_0.png)

