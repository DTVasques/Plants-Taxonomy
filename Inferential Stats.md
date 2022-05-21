
# Inferential tests

## Shapiro test of normality
> shapiro.test(FILE.csv$VARIABLE)

## Difference significance tests

Add the lines below to the ggplot line:
  
  > stat_compare_means(method = "wilcox.test")

> stat_compare_means(method = "t.test")

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
