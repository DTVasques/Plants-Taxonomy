#Method 2 - https://www.r-spatial.org/r/2018/10/25/ggplot2-sf-2.html
install.packages("cowplot")
install.packages("googleway")
install.packages("ggplot2")
install.packages("ggrepel")
install.packages("ggspatial")
install.packages("libwgeom")
install.packages("sf")
install.packages("rnaturalearth")
install.packages("rnaturalearthdata")

library("ggplot2")
theme_set(theme_bw())
library("sf")

library("rnaturalearth")
library("rnaturalearthdata")

#Plotting the data
world <- ne_countries(scale = "medium", returnclass = "sf")
class(world)

#Not using
(sites <- data.frame(longitude = c(-100, -90), latitude = c(20,50)))

#using
ggplot(data = world) +
  geom_sf() +
  geom_point(data = fujisanense, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Red") +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE) + ggtitle('H. fujisanense') 

ggplot(data = world) +
  geom_sf() +
  geom_point(data = Japanese, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Purple") +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE) + ggtitle('Sampled') 

ggplot(data = world) +
  geom_sf() +
  geom_point(data = punctissorum, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Red") +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE) + ggtitle('H. punctisorum') 

ggplot(data = world) +
  geom_sf() +
  geom_point(data = parallelocarpum, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Red") +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE) + ggtitle('H. parallelocarpum') 

#Merged map

ggplot(data = world) +
  geom_sf() +
  geom_point(data = parallelocarpum, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Red") +
  geom_point(data = punctissorum, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Blue") +
  geom_point(data = fujisanense, aes(x = x, y = y), size = 2, 
             shape = 21, fill = "Orange") +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE)

ggplot(data = world) +
  geom_sf() +
  geom_point(data = parallelocarpum, aes(x = x, y = y), size = 3, 
             shape = 21, fill = "Red") +
  geom_point(data = punctissorum, aes(x = x, y = y), size = 2, 
             shape = 22, fill = "Blue") +
  geom_point(data = fujisanense, aes(x = x, y = y), size = 2, 
             shape = 24, fill = "Orange") +
  coord_sf(xlim = c(127, 135), ylim = c(30, 35), expand = FALSE)


ggplot(data = world) +
  geom_sf() +
  geom_point(data = All_, aes(x = x, y = y), size = 2, 
             shape = 21, fill=Group) +
  coord_sf(xlim = c(125, 160), ylim = c(20, 50), expand = FALSE)
