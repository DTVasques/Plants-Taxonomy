# Creating Heatmaps in R

Reference: http://marianattestad.com/blog

# Step 1) Install packages and libraries

> if (!requireNamespace("BiocManager", quietly = TRUE))
>> install.packages("BiocManager")
>> BiocManager::install("ComplexHeatmap")

> library(ComplexHeatmap)

# Step 2) Read the data file
> test

> head(test)

> dim(test)

# Step 3) Select the collumns for the matrix
>my_matrix <- as.matrix(test [ ,c(2:5)])

>class(test)

>class(my_matrix)

# Step 4) Make a dataframe for later annotation
> annotation <- data.frame(country = test$...1)

> annotation

# Step5) Make the heatmap (default parameters)
> Heatmap(my_matrix)

# Step 6)Flip rows and columns
> my_matrix <- t(my_matrix)

> Heatmap(my_matrix)

## Keep bins in order, not clustered
> Heatmap(my_matrix, cluster_columns=FALSE, cluster_rows = FALSE)

## Change the fontsize
> fontsize <- 0.6

## Put cell labels on the left side

> Heatmap(my_matrix,
        row_names_side = 'left',
        row_names_gp=gpar(cex=fontsize),
        bottom_annotation = HeatmapAnnotation(df = annotation), show_column_names = annotation)

        
