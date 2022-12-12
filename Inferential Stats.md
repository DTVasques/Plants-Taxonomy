
# Inferential tests

## Shapiro test of normality
```js
shapiro.test(FILE.csv$VARIABLE)
```

## Difference significance tests

Add the lines below to the ggplot line:

```js
stat_compare_means(method = "wilcox.test")
stat_compare_means(method = "t.test")
```

## ANOVA

```js
Assumptions:
  1) Independent observations

2) Normal distribution (Shapiro test -> if p < 0.05 -> not normal)
> ggdensity (FILE.csv$VARIABLE, main = "TITLE", xlab = "Variable")

> ggqqplot(FILE.csv$VARIABLE)

> shapiro.test(FILE$VARIABLE)

3) Homogeneity of variance
> bartlett.test(FILE.csv$VARIABLE1 ~ FILE.csv$GROUP, data = df)
```

## Modality test

# Hartigan's Dip Test Statistic for Unimodality
```js
install.packages("diptest")
install.packages("LaplacesDemon")
library(diptest)
library(LaplacesDemon)
```

```js
dip.test(VARIABLE)
#data:  FILE$VARIABLE
#D = 0.047377, p-value = 0.8041
#alternative hypothesis: non-unimodal, i.e., at least bimodal

is.unimodal(VARIABLE)
#[1] TRUE
```

# Assessing Bimodality and Trimodality for the FALSE traits - https://universeofdatascience.com/how-to-determine-if-data-are-unimodal-or-multimodal-in-r/
```js
install.packages("LaplacesDemon")
install.packages("mousetrap")
library(LaplacesDemon)
library(mousetrap)
```

```js
bimodality_coefficient(VARIABLE)
#[1] 0.4885745
is.bimodal(VARIABLE)
#[1] TRUE
is.trimodal(VARIABLE)
#[1] FALSE
```


# Obtaing modes from multimodal data
```js
install.packages("LaplacesDemon")
library(LaplacesDemon)
```

```js
Modes(VARIABLE)$modes
# [1] 35.49813 54.79950
```


