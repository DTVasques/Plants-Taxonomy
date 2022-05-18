# Drawing distribution maps using ggplot 

Reference: https://www.r-spatial.org/r/2018/10/25/ggplot2-sf-2.html

Prepare your file in a csv format. The file we will be using here is called 'FILE.csv'. The file contains GPS (latitude and longitude) data for samples categorized in three variables: var1, var2 and var3.
[LDA.pdf](https://github.com/DTVasques/Plants-Taxonomy/files/8712975/LDA.pdf)

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
```

# Step 3) Plotting the data
> world <- ne_countries(scale = "medium", returnclass = "sf")
class(world)

> ggplot(data = world) +
  geom_sf() +
  geom_point(data = FILE.csv, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Red") +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE) + ggtitle('TITLE') 

# Step 4) Merging map for multiple variables

>ggplot(data = world) +
  geom_sf() +
  geom_point(data = FILE.csv$var1, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Red") +
  geom_point(data = FILE.csv$var2, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Blue") +
  geom_point(data = FILE.csv$var3, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Orange") +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE)
  
<img width="793" alt="Screen Shot 2022-05-18 at 12 03 44" src="https://user-images.githubusercontent.com/62867510/168948668-4a840889-5787-4a3a-977f-f6442b2d17b2.png">

