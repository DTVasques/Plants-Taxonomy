# Install libraries

library("ggplot2")
theme_set(theme_bw())
library("sf")
library("rnaturalearth")
library("rnaturalearthdata")
library(rgbif)
library(scrubr)
library(maps)
library ("tidyr")
library("dplyr")
library ("tidyverse")

```js
myspecies <- c("Plagiogyria")

gbif_data <- occ_data(scientificName = myspecies, hasCoordinate = TRUE, country = "JP", limit = 10000)  

gbif_data
```

```js
myspecies_coords <- gbif_data$data[ , c("decimalLongitude", "decimalLatitude", "species", "individualCount", "occurrenceStatus", "coordinateUncertaintyInMeters", "institutionCode", "references")]
head(myspecies_coords)

myspecies_coords_list <- vector("list", length(myspecies))
```

# Cleaning the dataset
# Remove records of absence or zero-abundance (if any)
```js
names(myspecies_coords)
sort(unique(myspecies_coords$individualCount))  # notice if some points correspond to zero abundance
sort(unique(myspecies_coords$occurrenceStatus))  # check for different indications of "absent", which could be in different languages! and remember that R is case-sensitive
absence_rows <- which(myspecies_coords$individualCount == 0 | myspecies_coords$occurrenceStatus %in% c("absent", "Absent", "ABSENT", "ausente", "Ausente", "AUSENTE"))
length(absence_rows)
if (length(absence_rows) > 0) {
  myspecies_coords <- myspecies_coords[-absence_rows, ]
}
# let's do some further data cleaning with functions of the 'scrubr' package (but note this cleaning is not exhaustive!)
nrow(myspecies_coords)
myspecies_coords <- coord_incomplete(coord_imprecise(coord_impossible(coord_unlikely(myspecies_coords))))
nrow(myspecies_coords)
```



#Remove NA from species column
```js
myspecies_coords_na <- myspecies_coords %>% drop_na(species)
```

#Correct mispells
```js
test <- myspecies_coords_na <- myspecies_coords_na %>% 
  mutate(species_C = case_when(
    species %in% "Plagiogyria matsumurana" ~ "Plagiogyria matsumureana"
    ,species %in%"Plagiogyria sessifolia" ~ "Plagiogyria sessilifolia"
    ,TRUE ~ species
  )
  )
  ```
  
# Plot the map
```js
map("world", xlim = range(myspecies_coords$decimalLongitude), ylim = range(myspecies_coords$decimalLatitude))  # if the map doesn't appear right at first, run this command again
points(myspecies_coords[ , c("decimalLongitude", "decimalLatitude")], pch = ".")


ggplot(data = world) +
  geom_sf() +
  geom_point(data = test, aes(x = decimalLongitude, y = decimalLatitude, fill = factor(species_C), color = NULL), size = 1, 
             shape = 21) +
  coord_sf(xlim = c(120, 150), ylim = c(20, 50)) 
```

