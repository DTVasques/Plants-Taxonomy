# Morphometrics using ggplot2

## Install packages
```js
library(readr)
library(tidyverse)
library(ggpubr)
theme_set(theme_pubr())
library ("ggplot2")
library ('ggfortify')
```

## Read the data file
```js
data <- read.csv("FILE.csv")
```

## Describe the data
```js
data %>% count(group, wt = NULL, sort = FALSE, name = NULL)
data %>% count(ploidy, wt = NULL, sort = FALSE, name = NULL)
```

## Plot a boxplot for each variable, according to the target groups
```js
ggplot(data = data, aes(x=ploidy, y = SterPetLength, fill=ploidy)) + geom_boxplot() + 
  labs(x="Ploidy", y="Petiole Length (cm)")+ geom_dotplot(binaxis='y', stackdir='center', position=position_dodge(1), dotsize = 0.5)+
  theme(legend.position = "none") 
```
<img width="654" alt="Screen Shot 2022-12-27 at 13 07 04" src="https://user-images.githubusercontent.com/62867510/209609515-fe121328-ce02-40e1-8c1b-ded0b24225a0.png">

## You can rearrange several plots in a single figure using 'ggarrange'
```js
ggarrange(a, b, c, d, ncol = 2, nrow = 2)


```
