# Morphometrics in R
You can use these commands when working with descriptive stats for morphometrics.
We will be comparing some mock data generated for individual of the fern species Hymenophyllum polyanthos.
The plants were measured according to the figure below:

<img width="317" alt="Screen Shot 2022-05-21 at 17 10 04" src="https://user-images.githubusercontent.com/62867510/169643056-b77495e4-2e05-4d9c-8150-a2e6f286a1c0.png">

Data file name: Data
Variables: Region, 'LL/LW', 'LL/PL', 'LL/ND', 'Pinnula angle', 'LPL/LPW'

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

## Counting data per group

Data %>% count("Region", wt = NULL, sort = FALSE, name = NULL)

```js
   Region freq
1       Asia   18
2 Neotropics   18
3    Pacific   18
```

## Summarizing data

ddply (Data, c("Region"), summarise, Mean = mean(`LL/LW`), SD = sd(`LL/LW`), MAX = max(`LL/LW`), MIN = min(`LL/LW`))

```js
Region     Mean        SD  MAX MIN
1       Asia 2.222222 1.5880271  8.0 0.4
2 Neotropics 2.322222 0.4659659  3.2 1.7
3    Pacific 5.422222 3.5122568 16.8 1.8
```

## Drawing boxplots
We can compare the variables variance and mean values between regions using the geom_boxplot() argument.
Let's compare the variable `LL/LW` between the three explores Regions:

> ggplot(data = Data, aes(x= Region, y = `LL/LW`)) + geom_boxplot() + 
  labs(x="Region", y="LL/LW")+ geom_dotplot(binaxis='y', stackdir='center', position=position_dodge(1), dotsize = 0.5)
  
<img width="392" alt="Screen Shot 2022-05-21 at 17 25 12" src="https://user-images.githubusercontent.com/62867510/169643079-9163fe17-bbe4-40a0-9f62-42996eeabb06.png">

# Drawing Violin plots
> ggplot (FILE.csv , aes(x=GROUP, y=PL, fill= GROUP)) + geom_violin() + theme (axis.text.x=element_blank())

<img width="566" alt="Screen Shot 2022-05-21 at 17 50 43" src="https://user-images.githubusercontent.com/62867510/169643992-e0e30ec1-11cb-4bee-9b80-c7dcc62185eb.png">

## Drawing scatterplots
Now, let's compare the covariance between variables `LL/LW` and `LL/ND`:

> ggplot(Data, aes(x= `LL/LW`, y= `LL/ND` , shape=Region, color=Region)) + geom_point(size=2)

<img width="390" alt="Screen Shot 2022-05-21 at 17 30 40" src="https://user-images.githubusercontent.com/62867510/169643214-4737aea7-64e4-4248-b265-a8ba1ee0a54f.png">

# Step 2) PCA and LDA plots 

We can also compare the covariance of more than two variables by performing a PCA (Principal Component Analysis) or a LDA (Linear Discriminant Analysis).

## PCA
Data selection:

> df <- Data [, 2:5]
> pca_res <- prcomp(df, scale. = TRUE)

Plotting the PCA:
> autoplot(pca_res, data = Data, colour = 'Region', loadings=TRUE, loadings.colour='black', loadings.label=FALSE, loadings.label.size=2, loadings.label.colour='black', frame = TRUE, frame.type = 'norm')

<img width="391" alt="Screen Shot 2022-05-21 at 17 36 10" src="https://user-images.githubusercontent.com/62867510/169643447-062cb820-1474-4872-879c-fb0653392ff1.png">

Framing clusters:
> autoplot(pca_res, data = Data, colour = 'Region', loadings=TRUE, loadings.colour='black', loadings.label=TRUE, loadings.label.size=2, loadings.label.colour='black', frame = TRUE, frame.type = 'norm')

<img width="389" alt="Screen Shot 2022-05-21 at 17 36 27" src="https://user-images.githubusercontent.com/62867510/169643452-37055e01-873d-4fce-a6c9-39c3b86e9b18.png">

## LDA 
Preparation
> library(tidyverse)
> library(caret)
> theme_set(theme_classic())
> library(MASS)

Select data
> data <- Data [, 1:5]

Split the data into training (80%) and test set (20%)
> set.seed(123)

> training.samples <- Data$Region %>%
  createDataPartition(p = 0.8, list = FALSE)
  
> train.data <- data[training.samples, ]

> test.data <- data[-training.samples, ]          

Estimate preprocessing parameters
> preproc.param <- train.data %>% 
  preProcess(method = c("center", "scale"))

Transform the data using the estimated parameters

> train.transformed <- preproc.param %>% predict(train.data)

> test.transformed <- preproc.param %>% predict(test.data)

Fit the model
> model <- lda(Region~., data = train.transformed)

Make predictions
> predictions <- model %>% predict(test.transformed)

Model accuracy
> mean(predictions$class==test.transformed$Region)

```js
[1] 0.6666667
```

Compute LDA
> model <- lda(Region~., data = train.transformed)

> model

```js
Call:
lda(Region ~ ., data = train.transformed)

Prior probabilities of groups:
      Asia Neotropics    Pacific 
 0.3333333  0.3333333  0.3333333 

Group means:
              `LL/LW`    `LL/PL`    `LL/ND` `Pinnula angle`
Asia       -0.3788001 -0.4497766 -0.3127133      0.05973894
Neotropics -0.4021828 -0.1877982 -0.2431799      0.90455455
Pacific     0.7809829  0.6375748  0.5558932     -0.96429349

Coefficients of linear discriminants:
                       LD1        LD2
`LL/LW`         -1.2661863 -0.7343952
`LL/PL`         -0.1001029 -0.9364680
`LL/ND`          0.7586180  0.5604085
`Pinnula angle`  1.3439841 -0.7203769

Proportion of trace:
  LD1   LD2 
0.946 0.054 
```

Plot
> plot(model)

<img width="490" alt="Screen Shot 2022-05-21 at 17 48 17" src="https://user-images.githubusercontent.com/62867510/169643818-f1afdf9f-0216-480c-bbc7-d2a29c7f4930.png">

make predictions
> predictions <- model %>% predict(test.transformed)
names(predictions)

Predicted classes
> head(predictions$class, 6)

Predicted probabilities of class memebership
> head(predictions$posterior, 6) 

Linear discriminants
> head(predictions$x, 3) 

```js
  LD1        LD2
1 -0.2097511  1.5458457
2  1.6252681 -0.1183713
3 -0.5515373  0.1503011
```

Plot in ggplot
> lda.data <- cbind(train.transformed, predict(model)$x)
> ggplot(lda.data, aes(LD1, LD2)) +
  geom_point(aes(color = Region))
  
  <img width="566" alt="Screen Shot 2022-05-21 at 17 50 43" src="https://user-images.githubusercontent.com/62867510/169643904-a3fb1f65-014e-4744-a90b-49e216b21399.png">
