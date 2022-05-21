# Morphometrics in R
You can use these commands when working with descriptive stats for morphometrics.

<img width="317" alt="Screen Shot 2022-05-21 at 17 10 04" src="https://user-images.githubusercontent.com/62867510/169643056-b77495e4-2e05-4d9c-8150-a2e6f286a1c0.png">

# Step 1) Install packages and download libraries
> install.packages("ggplot2")
install.packages("ggfortify")
install.packages("tidyverse")
install.packages("ggpubr")

> library ("ggplot2")
> library ('ggfortify')
> library(tidyverse)
> library (dplyr)
> library(ggpubr)
> theme_set(theme_pubr())

# Counting data per group

Data %>% count("Region", wt = NULL, sort = FALSE, name = NULL)

```js
   Region freq
1       Asia   18
2 Neotropics   18
3    Pacific   18
```

# Summarizing data

ddply (Data, c("Region"), summarise, Mean = mean(`LL/LW`), SD = sd(`LL/LW`), MAX = max(`LL/LW`), MIN = min(`LL/LW`))

```js
Region     Mean        SD  MAX MIN
1       Asia 2.222222 1.5880271  8.0 0.4
2 Neotropics 2.322222 0.4659659  3.2 1.7
3    Pacific 5.422222 3.5122568 16.8 1.8
```

# Draw boxplots
> ggplot(data = Data, aes(x= Region, y = `LL/LW`)) + geom_boxplot() + 
  labs(x="Region", y="LL/LW")+ geom_dotplot(binaxis='y', stackdir='center', position=position_dodge(1), dotsize = 0.5)
  
<img width="392" alt="Screen Shot 2022-05-21 at 17 25 12" src="https://user-images.githubusercontent.com/62867510/169643079-9163fe17-bbe4-40a0-9f62-42996eeabb06.png">


# PCA plot - data selection
df <- FILE.csv [, 1:10]
pca_res <- prcomp(df, scale. = TRUE)

autoplot(pca_res, data = FILE.csv, colour = 'GROUP', loadings=TRUE, loadings.colour='black', loadings.label=TRUE, loadings.label.size=2, loadings.label.colour='black', frame = FALSE, frame.type = 'norm')

# PCA summary
myPr$x

# Draw scatterplots

> ggplot(FILE.csv, aes(x= VARIABLE1, y= VARIABLE2 , shape=GROUP, color=GROUP)) + geom_point(size=2)

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
