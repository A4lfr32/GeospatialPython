# GeospatialPython

Recommend intro course: [Geospatial Analysis](https://www.kaggle.com/learn/geospatial-analysis)

* Reference systems
* Interactive maps
* Manipulation geo-data
* Proximity analysis

# Data
[¿Cómo descargar shapefile, data y mapas de Colombia? GRATIS](https://youtu.be/K0DVIyLOlzI)

[How to download Shapefile Data GIS for any country](https://youtu.be/18cj3VKg9gM)
---
# GeoPandas module
Installation:

    conda install geopandas
    pip install geopandas
``` python
import geopandas as gpd
```
Formats of data: shapefile, GeoJSON, KML, GPKG,...

``` python
# Read in the data
full_data = gpd.read_file("../input/geospatial-learn-course-data/DEC_lands/DEC_lands/DEC_lands.shp")

# View the first five rows of the data
full_data.head()

type(full_data)
#>> geopandas.geodataframe.GeoDataFrame

data = full_data.loc[:, ["CLASS", "COUNTY", "geometry"]].copy()

data.CLASS.value_counts()

# --
# Select lands that fall under the "WILD FOREST" or "WILDERNESS" category
wild_lands = data.loc[data.CLASS.isin(['WILD FOREST', 'WILDERNESS'])].copy()
wild_lands.head()
# --
wild_lands.plot()
# --
# View the first five entries in the "geometry" column
wild_lands.geometry.head()

```

``` python
# Define a base map with county boundaries
ax = counties.plot(figsize=(10,10), color='none', edgecolor='gainsboro', zorder=3)

# Add wild lands, campsites, and foot trails to the base map
wild_lands.plot(color='lightgreen', ax=ax)
campsites.plot(color='maroon', markersize=2, ax=ax)
trails.plot(color='black', markersize=1, ax=ax)
```

# CRS, Reference system
print(regions.crs)

``` python
# Create a DataFrame with health facilities in Ghana
facilities_df = pd.read_csv("../input/geospatial-learn-course-data/ghana/ghana/health_facilities.csv")

# Convert the DataFrame to a GeoDataFrame
facilities = gpd.GeoDataFrame(facilities_df, geometry=gpd.points_from_xy(facilities_df.Longitude, facilities_df.Latitude))

# Set the coordinate reference system (CRS) to EPSG 4326
facilities.crs = {'init': 'epsg:4326'}

# View the first five rows of the GeoDataFrame
facilities.head()
```
* The gpd.points_from_xy() function creates Point objects from the latitude and longitude columns.

``` python
# Create a map
ax = regions.plot(figsize=(8,8), color='whitesmoke', linestyle=':', edgecolor='black')
facilities.to_crs(epsg=32630).plot(markersize=1, ax=ax)
```

``` python
# The "Latitude" and "Longitude" columns are unchanged
facilities.to_crs(epsg=32630).head()

# ---
# Change the CRS to EPSG 4326
regions.to_crs("+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs").head()
```

``` python
# Get the x-coordinate of each point
facilities.geometry.head().x
```

``` python
# Calculate the area (in square meters) of each polygon in the GeoDataFrame 
regions.loc[:, "AREA"] = regions.geometry.area / 10**6

print("Area of Ghana: {} square kilometers".format(regions.AREA.sum()))
print("CRS:", regions.crs)
regions.head()
```

# Folium
# Geocoding
# Table joins
