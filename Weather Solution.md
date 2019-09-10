import pandas as pd
import numpy as np
import requests
import json
from citipy import citipy
import os as os
import matplotlib.pyplot as plt
import csv as csv


lat = np.random.uniform(low=-90.00, high=90.00, size=600)
lon = np.random.uniform(low=-180.00, high=180.00, size=600)


latlong = zip(lat, lon)


cities = []
for c in latlong:
    cities.append(citipy.nearest_city(c[0], c[1]))
  
  
city_name=[]
for city in cities:
    name = city.city_name
    city_name.append(name)


url = "http://api.openweathermap.org/data/2.5/weather?"
temps = []
humid = []
clouds = []
winds = []
lats = []
lons = []
names = []


for city in city_name:
    query_url = url + "appid=" + api_key + "&q=" + city + "&units=imperial"
    response = requests.get(query_url)
    if response.status_code == 200:
        response = response.json()
        temps.append(response["main"]["temp"])
        humid.append(response["main"]["humidity"])
        clouds.append(response["clouds"]["all"])
        winds.append(response["wind"]["speed"])
        lats.append(response["coord"]["lat"])
        lons.append(response["coord"]["lon"])
        names.append(response["name"])

weather = pd.DataFrame({"City": names,
                        "Temperature (F)": temps,
                        "Humidity (%)": humid,
                        "Cloud Coverage (%)": clouds,
                        "Wind Speed (mph)": winds,
                        "Latitude": lats, 
                        "Longitude": lons
                       })
weather.head()

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloud Coverage (%)</th>
      <th>Humidity (%)</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Punta Arenas</td>
      <td>75</td>
      <td>93</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>44.60</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Port Alfred</td>
      <td>88</td>
      <td>96</td>
      <td>-33.59</td>
      <td>26.89</td>
      <td>65.91</td>
      <td>10.74</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Alfred</td>
      <td>88</td>
      <td>96</td>
      <td>-33.59</td>
      <td>26.89</td>
      <td>65.91</td>
      <td>10.74</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Busselton</td>
      <td>76</td>
      <td>100</td>
      <td>-33.64</td>
      <td>115.35</td>
      <td>59.52</td>
      <td>24.16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>That Phanom</td>
      <td>88</td>
      <td>91</td>
      <td>16.94</td>
      <td>104.73</td>
      <td>75.18</td>
      <td>5.93</td>
    </tr>
  </tbody>
</table>
</div>

weather.plot.scatter(x="Latitude", y="Temperature (F)", title="Temperature per Latitude")



    <matplotlib.axes._subplots.AxesSubplot at 0x112f3e0b8>
    
weather.plot.scatter(x="Latitude", y="Humidity (%)", title="Humidity per Latitude")



    <matplotlib.axes._subplots.AxesSubplot at 0x1063029b0>

weather.plot.scatter(x="Latitude", y="Cloud Coverage (%)", title="Cloud Coverage per Latitude")



    <matplotlib.axes._subplots.AxesSubplot at 0x112ef25c0>

weather.plot.scatter(x="Latitude", y="Wind Speed (mph)", title="Wind Speed per Latitude")


