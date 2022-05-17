#This scrip brings commands for drawing PCA plots and violin plots in RStudio. You can use these commands when working with descriptive stats for morphometrics.

install.packages("ggplot2")
library ("ggplot2")

# PCA plot - data selection
myPr <- prcomp (Plantago_R[, 3:5], scale = TRUE)
# Eingen value plot
biplot (myPr, scale = 0)
#PCA summary
myPr$x
#PCA plot - graph
x <- cbind (Plantago_R, myPr$x [,1:2])
ggplot (x , aes(PC1, PC2, col = Group, fill = Group)) +  stat_ellipse(geom = "polygon", col = "black", alpha = 0.5) + geom_point (shape = 21, col = "black")


# Violin plots
ggplot (x , aes(x=Group, y=PL, fill= Group)) + geom_violin() + theme (axis.text.x=element_blank())
ggplot (x , aes(x=Group, y=BL, fill= Group)) + geom_violin() + theme (axis.text.x=element_blank())
ggplot (x , aes(x=Group, y=BW, fill= Group)) + geom_violin() + theme (axis.text.x=element_blank())
