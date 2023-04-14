# Drawing distribution maps using ggplot 

This tutorial will explain how to draw distribution maps using the ggplot package in R. 
We will be plotting occurrence data for Oxalis corniculata L. (Oxalidaceae) and Oxalis debilis Kunth. (Oxalidaceae) in Japan, and the data was obtained from GBIF.

```js
Data source: GBIF.org (18 May 2022) GBIF Occurrence Download  https://doi.org/10.15468/dl.upkjss

Code references: 
Source1: https://www.r-spatial.org/r/2018/10/25/ggplot2-sf-2.html
Source2: https://www.r-bloggers.com/2021/03/downloading-and-cleaning-gbif-data-with-r/

Documentation:
https://www.rdocumentation.org/packages/rgbif/versions/3.7.1/topics/occ_data
```

# Step 1) Install Packages

```js
install.packages("cowplot")
install.packages("googleway")
install.packages("ggplot2")
install.packages("ggrepel")
install.packages("ggspatial")
install.packages("libwgeom")
install.packages("sf")
install.packages("rnaturalearth")
install.packages("rnaturalearthdata")
```

# Step 2) Download libraries

```js
library("ggplot2")
theme_set(theme_bw())
library("sf")
library("rnaturalearth")
library("rnaturalearthdata")
library(rgbif)
```

# Step 3) Download and filter the occurrence data from GBIF

```js
myspecies <- c("Oxalis corniculata", "Oxalis debilis")

gbif_data <- occ_data(scientificName = myspecies, hasCoordinate = TRUE, country = "JP", limit = 2000)  
```

<img width="536" alt="Screen Shot 2022-05-18 at 12 50 08" src="https://user-images.githubusercontent.com/62867510/168953490-95898b4a-a164-4d5f-8859-cc0003d9d0ed.png">

Obs1: download GBIF occurrence data for these species; this may take a long time if there are many data points!

Obs2: "hasCoordinate" argument allows you to filter only data with GPS (latitude, longitude) information.

Obs3: "limit" argument allows you to select how many datapoints you wish to download from the records available.


## Take a look at the downloaded data:
```js
gbif_data
```

## Check how the data are organized:
```js
names(gbif_data)

names(gbif_data[[myspecies[1]]])

names(gbif_data[[myspecies[1]]]$meta)

names(gbif_data[[myspecies[1]]]$data)
```

## Create and fill a list with only the 'data' section for each species:
```js
myspecies_coords_list <- vector("list", length(myspecies))

names(myspecies_coords_list) <- myspecies

for (s in myspecies) {
  coords <- gbif_data[[s]]$data[ , c("decimalLongitude", "decimalLatitude", "individualCount", "occurrenceStatus", "coordinateUncertaintyInMeters", "institutionCode", "references")]

myspecies_coords_list[[s]] <- data.frame(species = s, coords)
}

lapply(myspecies_coords_list, head)
```

# Collapse the list into a data frame:
```js
myspecies_coords <- as.data.frame(do.call(rbind, myspecies_coords_list), row.names = 1:sum(sapply(myspecies_coords_list, nrow)))

head(myspecies_coords)

tail(myspecies_coords)
```

# Step 4) Plotting the data
```js
world <- ne_countries(scale = "medium", returnclass = "sf")

class(world)

ggplot(data = world) +
  geom_sf() +
  geom_point(data = myspecies_coords, aes(x = decimalLongitude, y = decimalLatitude, fill = factor(species)), size = 2, 
             shape = 21) +
  coord_sf(xlim = c(120, 150), ylim = c(20, 50)) + ggtitle('Katabami') 
```
 
<img width="492" alt="Screen Shot 2022-05-18 at 12 50 54" src="https://user-images.githubusercontent.com/62867510/168953524-3ed230ab-3b56-457f-be85-8aef0c783b27.png">

# Step5) Further edit the plots colors and shapes

```js
ggplot(data = world) + geom_sf() + geom_point(data = myspecies_coords, aes(x = decimalLongitude, y = decimalLatitude, shape = species, color = species, fill = species), size = 2) + 
  scale_shape_manual(values=c(4, 21)) +
  scale_color_manual(values=c('black', 'red')) +
  scale_fill_manual(values=c('black', 'red')) +
  coord_sf(xlim = c(125, 150), ylim = c(25, 50)) + ggtitle('Katabami') 
```

<img width="456" alt="Screen Shot 2022-12-07 at 12 06 43" src="https://user-images.githubusercontent.com/62867510/206078430-4cd2c74e-e799-4faf-baef-470ba915b683.png">


![image](https://user-images.githubusercontent.com/62867510/231994669-c87dee01-c0ca-45c2-b75d-6902165167d6.png)


