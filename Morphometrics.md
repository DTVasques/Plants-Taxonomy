# Morphometrics in R
This scrip brings commands for drawing PCA plots and violin plots in RStudio. You can use these commands when working with descriptive stats for morphometrics.

# Step 1) Install packages and download libraries
> install.packages("ggplot2")
install.packages("ggfortify")
install.packages("tidyverse")
install.packages("ggpubr")

> library ("ggplot2")

> library ('ggfortify')

> library(tidyverse)

> library (plyr)

> library(ggpubr)

> theme_set(theme_pubr())

# Count data

FILE.csv %>% count(GROUP, wt = NULL, sort = FALSE, name = NULL)

# Summarize data

ddply (FILE.csv, c("GROUP"), summarise, N = variable(VARIABLE), Mean = mean(VARIABLE), SD = sd(VARIABLE), MAX = max(VARIABLE), MIN = min(VARIABLE))

# PCA plot - data selection
df <- FILE.csv [, 1:10]
pca_res <- prcomp(df, scale. = TRUE)

autoplot(pca_res, data = FILE.csv, colour = 'GROUP', loadings=TRUE, loadings.colour='black', loadings.label=TRUE, loadings.label.size=2, loadings.label.colour='black', frame = FALSE, frame.type = 'norm')

# PCA summary
myPr$x

# Draw scatterplots

> ggplot(FILE.csv, aes(x= VARIABLE1, y= VARIABLE2 , shape=GROUP, color=GROUP)) + geom_point(size=2)

# Draw boxplots
> ggplot(data = FILE.CSV, aes(x= GROUP, y = VARIABLE)) + geom_boxplot() + 
  labs(x="Group", y="Variable)")+ geom_dotplot(binaxis='y', stackdir='center', position=position_dodge(1), dotsize = 0.5)

# LDA 
## Step1) Preparation
> library(tidyverse)

> library(caret)

> theme_set(theme_classic())

### Select data
> data <- FILE.csv[c(1,2,3,4)]

> view(data)

### Split the data into training (80%) and test set (20%)
> set.seed(123)

> training.samples <- data$GROUP %>%
  createDataPartition(p = 0.8, list = FALSE)
  
> train.data <- data[training.samples, ]

> test.data <- data[-training.samples, ]          

### Estimate preprocessing parameters
> preproc.param <- train.data %>% 
  preProcess(method = c("center", "scale"))

### Transform the data using the estimated parameters

> train.transformed <- preproc.param %>% predict(train.data)

> test.transformed <- preproc.param %>% predict(test.data)

## Step 2) Estimation
> library(MASS)

### Fit the model
> model <- lda(GROUP~., data = train.transformed)

### Make predictions
> predictions <- model %>% predict(test.transformed)

### Model accuracy
> mean(predictions$class==test.transformed$GROUP)

## Step 3) Compute LDA
> model <- lda(GROUP~., data = train.transformed)

> model

### Plot
> plot(model)

### make predictions
> predictions <- model %>% predict(test.transformed)
names(predictions)

### Predicted classes
> head(predictions$class, 6)

### Predicted probabilities of class memebership
> head(predictions$posterior, 6) 

### Linear discriminants
> head(predictions$x, 3) 

### Plot in ggplot
> lda.data <- cbind(train.transformed, predict(model)$x)
ggplot(lda.data, aes(LD1, LD2)) +
  geom_point(aes(color = GROUP))

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

# Violin plots
> ggplot (FILE.csv , aes(x=GROUP, y=PL, fill= GROUP)) + geom_violin() + theme (axis.text.x=element_blank())
