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
```{r, echo=FALSE}
unzip("c:/r/week 2 assign 2/repdata-data-activity.zip")
dact<-("c:/r/week 2 assign 2/activity.csv")
assign2<-read.csv(dact,head=TRUE,sep=",")
hist(assign2$steps)
```
Use the base plot system to generate a histogram.
The histogram shows that the data are skewed

###summarize mean by day

library(plyr)
assign2mean<-ddply(assign2,c("date"),summarize,mean.steps=mean(steps,na.rm=TRUE))
head(assign2mean)
```{r, echo=FALSE}

library(plyr)
assign2mean<-ddply(assign2,c("date"),summarize,mean.steps=mean(steps,na.rm=TRUE))
head(assign2mean)
```
In this example use of plyr is made to easily summarize the data
The results show value of the mean by day
There are several NA in the data

###total number of steps per day

assign2sum<-ddply(assign2,c("date"),summarize,sum.steps=sum(steps,na.rm=TRUE))
hist(assign2sum$sum.steps)

```{r, echo=FALSE}

assign2sum<-ddply(assign2,c("date"),summarize,sum.steps=sum(steps,na.rm=TRUE))
hist(assign2sum$sum.steps)
```


The plot was created using the base plot system
It shows the total number of steps per day

###mean and median of steps per day
assign2meanmed<-ddply(assign2,c("date"),
                        summarize,
                                mean.steps=mean(steps,na.rm=TRUE),
                                median.steps=median(steps,na.rm=TRUE))
                                
```{r, echo=FALSE}
assign2meanmed<-ddply(assign2,c("date"),
                        summarize,
                                mean.steps=mean(steps,na.rm=TRUE),
                                median.steps=median(steps,na.rm=TRUE))
head(assign2meanmed)
```


I have included a header of the output 
This shows that I was unable to calculate the median correctly

###time series plot of average time of steps taken
assign2mean<-ddply(assign2,c("interval"),summarize,mean.steps=mean(steps,na.rm=TRUE))
 p<-ggplot(assign2mean,aes(x=interval,y=mean.steps,xlab="5 min Intervals",ylab="Average number of steps"))
p+geom_line()

```{r, echo=FALSE}
library(ggplot2)
assign2mean<-ddply(assign2,c("interval"),summarize,mean.steps=mean(steps,na.rm=TRUE))
 p<-ggplot(assign2mean,aes(x=interval,y=mean.steps,xlab="5 min Intervals",ylab="Average number of steps"))
p+geom_line()

```


Ggplot was used to generate the graph above which displays a time series of steps taken per interval

###total number of steps per interval
assign2sum<-ddply(assign2,c("interval"),summarize,sum.steps=sum(steps,na.rm=TRUE))
``` {r,echo=FALSE}
assign2sum<-ddply(assign2,c("interval"),summarize,sum.steps=sum(steps,na.rm=TRUE))
head(assign2sum)
```

Use was made of the plyr package to calculate the number of steps per interval

###report the number of missing values
I ran out of time to do this part of the assignment

###imputing data
library(Amelia)
a.out<-amelia(assign2,idvars="date")
```{r,echo=FALSE}
library(Amelia)
a.out<-amelia(assign2,idvars="date")
```

The amelia package had an easy syntax
Use was made of the package to impute missing data

###new dataset with missing values filled in
assign2.i<-a.out$imputations[[5]]
head(assign2.i)

```{r,echo=FALSE}
assign2.i<-a.out$imputations[[5]]
head(assign2.i)

```

It was decided to use the fifth imputation of data generated from amelia

###histogram of number of steps taken
 hist(assign2.i$steps)
 
```{r,echo=FALSE}
  hist(assign2.i$steps)
  hist(assign2$steps)

```

A histogram of the number of steps taken from the dataset with the imputed data is displayed
This is different from the original dataset. The graph on the top displays the imputed dataset and that on the bottom the dataset with the missing values. Data imputation reduced the skew in the data.
 

###report the mean and median of number of steps taken in imputed dataset
assign2.1meanmed<-ddply(assign2.i,c("date"),
+                       summarize,
+                       mean.steps=mean(steps,na.rm=TRUE),
+                       median.steps=median(steps,na.rm=TRUE))
head(assign2.imeanmed}

```{r, echo=FALSE}
assign2.i<-a.out$imputations[[5]]
assign2meanmed.i<-ddply(assign2.i,c("date"),
                        summarize,
                                mean.steps=mean(steps,na.rm=TRUE),
                                median.steps=median(steps,na.rm=TRUE))
head(assign2meanmed)
```
Again I was unable to correctly calculate the median.

```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
