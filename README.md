

A manual to show the R package `TippingPoint`.

## Introduction

The `TippingPoint` R package aims to handle missing value for outcomes by listing all the possible combinations of missing values in two treatment arms, calculating the estimated treatment effects and p-values, finding the margin (the so-called Tipping Point) that would change the result of a study, and drawing a heat-map to visualize them. In addition, the package provides a visualized method to compare various imputation methods by adding the rectangles or convex hulls on the basic plot. Some examples are displayed below:




```r
# the package can be downloaded from cran and github:

## install.packages("TipingPoint")

devtools::install_github("XikunHan/TippingPoint")

library(TippingPoint)

# Load the dataset

data(tippingdata)

# Show the first 6 rows of the data

head(tippingdata)

```


## Basic plot


```r
## for binary outcome


# Using `estimate`
TippingPoint(outcome=tippingdata$binary,
             treat= tippingdata$treat,group.infor=TRUE,
             plot.type = "estimate",ind.values = TRUE,
             impValuesT  = NA,  impValuesC = NA,
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(0.38,0.4), HistMeanC =  c(0.2,0.55))

# Using `p.value` with formula class
TippingPoint(binary~treat, data=tippingdata,
             plot.type = "p.value",ind.values = TRUE,
             impValuesT  = NA,  impValuesC = NA,
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(0.38,0.4), HistMeanC =  c(0.2,0.55))

# Using `both` 
TippingPoint(outcome=tippingdata$binary,treat= tippingdata$treat,
             plot.type = "both",ind.values = TRUE,
             impValuesT  = NA,  impValuesC = NA,
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(0.38,0.4), HistMeanC =  c(0.2,0.55))

# for continuous outcome
TippingPoint(continuous~treat, data=tippingdata,
             group.infor=TRUE, plot.type = "estimate",ind.values = TRUE,
             impValuesT  = NA,  impValuesC = NA,
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(120), HistMeanC =  c(131,137))

TippingPoint(outcome=tippingdata$continuous,treat= tippingdata$treat,
             plot.type = "p.value",ind.values = TRUE,
             impValuesT  = NA,  impValuesC = NA,
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(120), HistMeanC =  c(131,137))

TippingPoint(outcome=tippingdata$continuous,treat= tippingdata$treat,
             plot.type = "both",ind.values = TRUE,
             impValuesT  = NA,  impValuesC = NA,
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(120), HistMeanC =  c(131,137))

```

## Using imputed data

```r
# Load the imputed dataset

data(imputedata)

# Show the first 6 rows of the data

head(imputedata)


## for binary outcome

TippingPoint(outcome=tippingdata$binary,
             treat= tippingdata$treat, group.infor=TRUE,
             plot.type = "estimate",ind.values = TRUE,
             impValuesT  = imputedata[,c("MAR_T2","MCAR_T2")],  
             impValuesC = imputedata[,c("MAR_C2","MCAR_C2")],
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(0.38,0.4), HistMeanC =  c(0.2,0.55))


# User-defined colors
TippingPoint(outcome=tippingdata$binary,treat= tippingdata$treat,
             plot.type = "p.value",ind.values = TRUE,
             impValuesT  = imputedata[,c("MAR_T2","MCAR_T2")],  
             impValuesC = imputedata[,c("MAR_C2","MCAR_C2")],
             impValuesColor = RColorBrewer::brewer.pal(8,"Accent")[5:6],
             summary.type = "credible.region", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(0.38,0.4), HistMeanC =  c(0.2,0.55))

# Using `point.size` and `point.shape` to control the points.
TippingPoint(outcome=tippingdata$binary,treat= tippingdata$treat,
             plot.type = "both",ind.values = TRUE,
             impValuesT  = imputedata[,c("MAR_T2","MCAR_T2")],  
             impValuesC = imputedata[,c("MAR_C2","MCAR_C2")],
             impValuesColor =c("red","blue"),
             point.size=0.8,point.shape = 15,
             summary.type = "convex.hull", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(0.38,0.4), HistMeanC =  c(0.2,0.55))



## for continuous outcome
TippingPoint(outcome=tippingdata$continuous,
             treat= tippingdata$treat, group.infor=TRUE,
             plot.type = "p.value",ind.values = TRUE,
             impValuesT  = imputedata[,c("MAR_T1","MCAR_T1")],  
             impValuesC = imputedata[,c("MAR_C1","MCAR_C1")],
             summary.type = "density", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(120), HistMeanC =  c(131,137))


# Using `point.size` and `point.shape` to control the points.
TippingPoint(outcome=tippingdata$continuous,treat= tippingdata$treat,
             plot.type = "p.value",ind.values = TRUE,
             impValuesT  = imputedata[,c("MAR_T1","MCAR_T1")],  
             impValuesC = imputedata[,c("MAR_C1","MCAR_C1")],
             point.size = 0.8, point.shape = 15,
             summary.type = "credible.region", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(120), HistMeanC =  c(131,137))

TippingPoint(outcome=tippingdata$continuous,treat= tippingdata$treat,
             plot.type = "both",ind.values = TRUE,
             impValuesT  = imputedata[,c("MAR_T1","MCAR_T1")],  
             impValuesC = imputedata[,c("MAR_C1","MCAR_C1")],
             summary.type = "convex.hull", alpha = 0.95, S=1.5, n.grid = 100,
             HistMeanT = c(120), HistMeanC =  c(131,137))


```



## Reference

The `TippingPoint` package is created based on Liublinska and Rubin's work. For more details:

* Liublinska, V. and Rubin, D.B. Enhanced Tipping-Point Displays. In JSM Proceedings, Section on Survey Research Methods, San Diego, CA: American Statistical Association. 3861-3686 (2012).
* Liublinska, V. & Rubin, D.B. Sensitivity analysis for a partially missing binary outcome in a two-arm randomized clinical trial. Stat Med 33, 4170-85 (2014).
* Liublinska, V. (May, 2013) Sensitivity Analyses in Empirical Studies Plagued with Missing Data. PhD Dissertation, Harvard University, https://dash.harvard.edu/handle/1/11124841
* https://sites.google.com/site/vliublinska/research


