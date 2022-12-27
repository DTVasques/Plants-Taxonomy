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

## ANOVA test

# Edit from here
```js
dat <- data
x <- which(names(dat) == "ploidy") # name of grouping variable
y <- which(names(dat) == "SterPetLength" # names of variables to test
           | names(dat) == "SterBladeLength" |
             names(dat) == "SterBladeWidth" |
             names(dat) == "PinnaNumber")
method1 <- "anova" # one of "anova" or "kruskal.test"
method2 <- "t.test" # one of "wilcox.test" or "t.test"
my_comparisons <- list(c("4x", "5x"), c("4x", "6x"), c("5x", "6x")) # comparisons for post-hoc tests
# Edit until here


# Edit at your own risk
for (i in y) {
  for (j in x) {
    p <- ggboxplot(dat,
                   x = colnames(dat[j]), y = colnames(dat[i]),
                   color = colnames(dat[j]),
                   legend = "none",
                   palette = "npg",
                   add = "jitter"
    )
    print(
      p + stat_compare_means(aes(label = paste0(..method.., ", p-value = ", ..p.format..)),
                             method = method1, label.y = max(dat[, i], na.rm = TRUE)
      )
      + stat_compare_means(comparisons = my_comparisons, method = method2, label = "p.format") # remove if p-value of ANOVA or Kruskal-Wallis test >= alpha
    )
  }
}
```

