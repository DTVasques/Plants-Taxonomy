#Environmental data (dismo package)

library(dismo)
files <- list.files(path=paste(system.file(package="dismo"), '/ex', sep=''),
                    pattern='grd', full.names=TRUE )

predictors <- stack(files)
predictors
names(predictors)
plot(predictors)

> # first layer of the RasterStack
  plot(predictors, 1)

> # note the "add=TRUE" argument with plot
  plot(wrld_simpl, xlim=c,(0, -40), ylim=c(-50,50), add=TRUE)

> # with the points function, "add" is implicit > 
  points(Adiantum$Longitude, Adiantum$Latitude, col='red', pch=20, cex=0.25) 
points(Vittaria$Longitude, Vittaria$Latitude, col='red', pch=20, cex=0.25) 

#values
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

#The example above uses data representing ’bioclimatic variables’ from the WorldClim database (http://www.worldclim.org, Hijmans et al., 2004) and ’terrestrial biome’ data from the WWF. (http://www.worldwildlife.org/science/data/item1875.html, Olsen et al., 2001). You can go to these web- sites if you want higher resolution data. You can also use the getData function from the raster package to download WorldClim climate data.

#Get data from other countries

# First, find the acronym for the country
getData('ISO3') 

#'alt' (elevation data) aggregated from SRTM 90m resolution data between -60 and 60 latitude.
#'GADM' is a database of global administrative boundaries. 
#'worldclim' is a database of global interpolated climate data.
#'SRTM' refers to the hole-filled CGIAR-SRTM (90 m resolution). 
#level = (0=country, 1=first level subdivision)

#If name is 'alt' or 'GADM' you must provide a 'country=' argument. Countries are specified by their 3 letter ISO codes. Use getData('ISO3') to see these codes. In the case of GADM you must also provide the level of administrative subdivision (0=country, 1=first level subdivision). In the case of alt you can set 'mask' to FALSE. If it is TRUE values for neighbouring countries are set to NA.
#If name is 'SRTM' you must provide 'lon' and 'lat' arguments (longitude and latitude). These should be single numbers somewhere within the SRTM tile that you want.
#If name='worldclim' you must also provide arguments var, and a resolution res. Valid variables names are 'tmin', 'tmax', 'prec' and 'bio'. Valid resolutions are 0.5, 2.5, 5, and 10 (minutes of a degree). In the case of res=0.5, you must also provide a lon and lat argument for a tile; for the lower resolutions global data will be downloaded. In all cases there are 12 (monthly) files for each variable except for 'bio' which contains 19 files.
#To get (projected) future climate data (CMIP5), you must provide arguments var and res as above. Only resolutions 2.5, 5, and 10 are currently available. In addition, you need to provide model, rcp and year. 

worldclim <- getData('worldclim', var='bio', res=10)
plot(worldclim$bio1, , xlim=c(120, 150), ylim=c(20,50))
plot(wrld_simpl, add=TRUE)


plot(bio1)

GADM <- getData('GADM', country='JPN', level=1)

plot(GADM)
