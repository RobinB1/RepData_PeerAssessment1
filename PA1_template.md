---
title: "PA1_template.md"
author: "Robin"
date: "2 December 2016"
output:
  html_document: default
  pdf_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
options(scipen = 999)
library(knitr)
library(markdown)
library(lattice)
```
## Get File, download then create base dataset
```{r Activity_Data, echo=FALSE}
 Activity_Data <- read.csv("C:/Users/Robin/datasciencecoursera/data/activity.csv", header = TRUE, sep = ",", na.strings="NA")
```
## Total steps by day
```{r Steps_day, Create Total Steps by Day}
Steps_Day <- aggregate(steps ~ date, Activity_Data, sum)
```
## Create Histogram of Total Steps per Day.
```{r plot 1, echo=TRUE}
hist(Steps_Day$steps, main = paste("Total Steps for Each Day"), col="blue", xlab="Number of Steps")

```

## Calculate the Mead and Median values for Steps taken per Day
```{r Mean and Median Values}
Mean_Steps <- mean(Steps_Day$steps)
Mean_Steps

Median_Steps <- median(Steps_Day$steps)
Median_Steps
```
Mean Steps = 10766.19 and Median Steps = 10765

## Create Steps Interval
```{r Steps Interval}
Steps_Interval <- aggregate(steps ~ interval, Activity_Data, mean)
```

## Plot Steps Interval
```{r plot 2, echo=TRUE}
plot(Steps_Interval$interval,Steps_Interval$steps, type="l", xlab="Interval", ylab="Number of Steps",main="Average Number of Steps per Day by Interval")
```

## Calculate Maximum number of Steps
```{r Maximum Steps}
Max_Interval <- Steps_Interval[which.max(Steps_Interval$steps),1]
Max_Interval

```
Maximum number of steps = 835

## Count number of N/As
```{r Count N/A}
ActData_NA <- sum(!complete.cases(Activity_Data))
ActData_NA

```
Total number of cases that were set as N/As = 2304

## Update N/As
```{r Update N/As}
Activity_Data2<- Activity_Data
Get_NAs <- is.na(Activity_Data2$steps)
Avg_Interval<- tapply(Activity_Data2$steps, Activity_Data2$interval, mean, na.rm=TRUE, simplify = TRUE)
Activity_Data2$steps[Get_NAs] <- Avg_Interval[as.character(Activity_Data2$interval[Get_NAs])]
```

## Reset Steps per Day to include updated N/As
```{r Activity_data2, Update Steps per Day}
Steps_Day <- aggregate(steps ~ date, Activity_Data2, sum)
```

## Recount Median and Mean Values
```{r Recount Mean and Median}
Mean_Steps <- mean(Steps_Day$steps)
Mean_Steps

Median_Steps <- median(Steps_Day$steps)
Median_Steps
```
Mean Steps = 10766.19 and Median Steps now equals 10766.19


## Re-Create plot1 to implement updated Median values
```{r plot 3, echo=TRUE}
hist(Steps_Day$steps, main = paste("Total Steps Each Day"), col="blue", xlab="Number of Steps")
```

## Creating a variable then plot to compare and contrast number of steps between the week and weekend.
## Examining if there is a higher peak earlier on weekdays, or more overall activity on weekends.
```{r Create Days}
weekdays <- c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday")

Activity_Data2$dow = as.factor(ifelse(is.element(weekdays(as.Date(Activity_Data2$date)),weekdays), "Weekday", "Weekend"))

Steps_Interval_i <- aggregate(steps ~ interval + dow, Activity_Data2, mean)
```

## Creating Plot for Days of Week
```{r DoW, echo=TRUE}
xyplot(Steps_Interval_i$steps ~ Steps_Interval_i$interval|Steps_Interval_i$dow, main="Average Steps per Day by Interval",
         xlab="Interval", ylab="Steps",layout=c(1,2), type="l")
```
