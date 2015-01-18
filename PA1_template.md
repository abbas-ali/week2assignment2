---
title: "Week Two Assignment Two"
author: "Abbas Ali"
date: "Sunday, January 18, 2015"
output: html_document
---

This is the solution to the knitr assighment and my first attempt at using knitr


```{r eval=TRUE, echo=TRUE


#Inputting the data


###unzip the file
unzip("c:/r/week 2 assign 2/repdata-data-activity.zip")


###set a name
 dact<-("c:/r/week 2 assign 2/activity.csv")

###read data file
assign2<-read.csv(dact,head=TRUE,sep=",")

###visualize the data

hist(assign2$steps)
![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1-1.png) 
Use the base plot system to generate a histogram.
The histogram shows that the data are skewed

###summarize mean by day

library(plyr)
assign2mean<-ddply(assign2,c("date"),summarize,mean.steps=mean(steps,na.rm=TRUE))
head(assign2mean)

```
##         date mean.steps
## 1 2012-10-01        NaN
## 2 2012-10-02    0.43750
## 3 2012-10-03   39.41667
## 4 2012-10-04   42.06944
## 5 2012-10-05   46.15972
## 6 2012-10-06   53.54167
```
In this example use of plyr is made to easily summarize the data
The results show value of the mean by day
There are several NA in the data

###total number of steps per day

assign2sum<-ddply(assign2,c("date"),summarize,sum.steps=sum(steps,na.rm=TRUE))
hist(assign2sum$sum.steps)

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 


The plot was created using the base plot system
It shows the total number of steps per day

###mean and median of steps per day
assign2meanmed<-ddply(assign2,c("date"),
                        summarize,
                                mean.steps=mean(steps,na.rm=TRUE),
                                median.steps=median(steps,na.rm=TRUE))
                                

```
##         date mean.steps median.steps
## 1 2012-10-01        NaN           NA
## 2 2012-10-02    0.43750            0
## 3 2012-10-03   39.41667            0
## 4 2012-10-04   42.06944            0
## 5 2012-10-05   46.15972            0
## 6 2012-10-06   53.54167            0
```


I have included a header of the output 
This shows that I was unable to calculate the median correctly

###time series plot of average time of steps taken
assign2mean<-ddply(assign2,c("interval"),summarize,mean.steps=mean(steps,na.rm=TRUE))
 p<-ggplot(assign2mean,aes(x=interval,y=mean.steps,xlab="5 min Intervals",ylab="Average number of steps"))
p+geom_line()

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 


Ggplot was used to generate the graph above which displays a time series of steps taken per interval

###total number of steps per interval
assign2sum<-ddply(assign2,c("interval"),summarize,sum.steps=sum(steps,na.rm=TRUE))

```
##   interval sum.steps
## 1        0        91
## 2        5        18
## 3       10         7
## 4       15         8
## 5       20         4
## 6       25       111
```

Use was made of the plyr package to calculate the number of steps per interval

###report the number of missing values
I ran out of time to do this part of the assignment

###imputing data
library(Amelia)
a.out<-amelia(assign2,idvars="date")

```
## Loading required package: Rcpp
## ## 
## ## Amelia II: Multiple Imputation
## ## (Version 1.7.3, built: 2014-11-14)
## ## Copyright (C) 2005-2015 James Honaker, Gary King and Matthew Blackwell
## ## Refer to http://gking.harvard.edu/amelia/ for more information
## ##
```

```
## -- Imputation 1 --
## 
##   1  2
## 
## -- Imputation 2 --
## 
##   1  2
## 
## -- Imputation 3 --
## 
##   1  2
## 
## -- Imputation 4 --
## 
##   1  2
## 
## -- Imputation 5 --
## 
##   1  2
```

The amelia package had an easy syntax
Use was made of the package to impute missing data

###new dataset with missing values filled in
assign2.i<-a.out$imputations[[5]]
head(assign2.i)


```
##       steps       date interval
## 1 179.56894 2012-10-01        0
## 2 -95.55382 2012-10-01        5
## 3  71.13919 2012-10-01       10
## 4 -31.14465 2012-10-01       15
## 5  60.21789 2012-10-01       20
## 6 -29.69540 2012-10-01       25
```

It was decided to use the fifth imputation of data generated from amelia

###histogram of number of steps taken
 hist(assign2.i$steps)
 
![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png) ![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-2.png) 

A histogram of the number of steps taken from the dataset with the imputed data is displayed
This is different from the original dataset. The graph on the top displays the imputed dataset and that on the bottom the dataset with the missing values. Data imputation reduced the skew in the data.
 

###report the mean and median of number of steps taken in imputed dataset
assign2.1meanmed<-ddply(assign2.i,c("date"),
+                       summarize,
+                       mean.steps=mean(steps,na.rm=TRUE),
+                       median.steps=median(steps,na.rm=TRUE))
head(assign2.imeanmed}


```
##         date mean.steps median.steps
## 1 2012-10-01        NaN           NA
## 2 2012-10-02    0.43750            0
## 3 2012-10-03   39.41667            0
## 4 2012-10-04   42.06944            0
## 5 2012-10-05   46.15972            0
## 6 2012-10-06   53.54167            0
```
Again I was unable to correctly calculate the median.

```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
