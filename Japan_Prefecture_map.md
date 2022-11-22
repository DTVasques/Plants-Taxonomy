data source: [data.csv](https://github.com/DTVasques/Plants-Taxonomy/files/10065038/data.csv)

> Data needs to be organized in two columns, named "region" and "value", respectively.
> The "region" variable should include the prefectures names.
> The "value" variable can include numerical data of any kind (in the dataset above, it includes the number of occurrences for *Cryptomeria* trees).

# Download libraries
library("choroplethr")
library("choroplethrAdmin1")
admin1_map("japan")

# Plot data

PlotData <- data.frame(region = data[, 1], value = data[, 2])

admin1_choropleth(country.name = "japan",
                  df           = PlotData,
                  title        = "TITLE",
                  legend       = "LEGEND",
                  1)
                  
 <img width="690" alt="Screen Shot 2022-11-22 at 16 49 25" src="https://user-images.githubusercontent.com/62867510/203255903-2e125fa2-eda0-4da5-84da-ed34d34bdd79.png">
    
