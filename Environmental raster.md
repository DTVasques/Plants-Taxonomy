# Adding Environmental data to maps (dismo package)

The data file 'FILE.csv' contain data on organisms and their latitude and longitude distribution.

Data sources:
> ’bioclimatic variables’ from the WorldClim database (http://www.worldclim.org, Hijmans et al., 2004)

> ’terrestrial biome’ data from the WWF (http://www.worldwildlife.org/science/data/item1875.html, Olsen et al., 2001)

# Step 1) Download and install libraries
library(dismo)

# Step 2) Prepare files
> files <- list.files(path=paste(system.file(package="dismo"), '/ex', sep=''),
                    pattern='grd', full.names=TRUE )

> predictors <- stack(files)

> predictors

> names(predictors)

> plot(predictors)

# First layer of the RasterStack
> plot(predictors, 1)

> plot(wrld_simpl, xlim=c,(0, -40), ylim=c(-50,50), add=TRUE)

# Plot occurrence points 
> points(FILE.csv$Longitude, FILE.csv$Latitude, col='red', pch=20, cex=0.25) 

# Getting data from other countries

# A) 'WordClim' 

Database of global interpolated climate data.

> worldclim <- getData('worldclim', var='bio', res=10)
> plot(worldclim$bio1, , xlim=c(120, 150), ylim=c(20,50))
> plot(wrld_simpl, add=TRUE)

```js
Provide arguments var, and a resolution res. 

Valid variables: 'tmin', 'tmax', 'prec' and 'bio'. 
Valid resolutions: 0.5, 2.5, 5, and 10 (minutes of a degree). 
* In the case of res=0.5: provide a lon and lat argument for a tile.
```

```js
'var' values
bio1 = Mean annual temperature
bio2 = Mean diurnal range (mean of max temp - min temp)
bio3 = Isothermality (bio2/bio7) (* 100)
bio4 = Temperature seasonality (standard deviation *100)
bio5 = Max temperature of warmest month
bio6 = Min temperature of coldest month
bio7 = Temperature annual range (bio5-bio6)
bio8 = Mean temperature of the wettest quarter
bio9 = Mean temperature of driest quarter
bio10 = Mean temperature of warmest quarter
bio11 = Mean temperature of coldest quarter
bio12 = Total (annual) precipitation
bio13 = Precipitation of wettest month
bio14 = Precipitation of driest month
bio15 = Precipitation seasonality (coefficient of variation)
bio16 = Precipitation of wettest quarter
bio17 = Precipitation of driest quarter
bio18 = Precipitation of warmest quarter
```

# B) 'GADM'

Database of global administrative boundaries. 

> GADM <- getData('GADM', country='JPN', level=1)

> plot(GADM)

```js
Provide a 'country=' argument. Countries are specified by their 3 letter ISO codes. Use getData('ISO3') to see these codes. 

Provide the level of administrative subdivision level = (0=country, 1=first level subdivision). 
```

# C) 'alt'

Elevation data aggregated from SRTM 90m resolution data between -60 and 60 latitude.

```js
Provide a 'country=' argument. Countries are specified by their 3 letter ISO codes. Use getData('ISO3') to see these codes. 

Set 'mask' to FALSE. If it is TRUE values for neighbouring countries are set to NA.
```

# D) 'SRTM'

Hole-filled CGIAR-SRTM (90 m resolution). 

```js
Provide 'lon' and 'lat' arguments (longitude and latitude)as single numbers.
```
