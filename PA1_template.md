---
title: "PA1_template.Rmd"
output: html_document
date: "2023-04-20"
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


> knitr::opts_chunk$set(echo = TRUE)
> library(ggplot2)

> fullData <- repdata_data_activity
> fullData$date <- as.Date(fullData$date, "%Y-%m-%d")

> stepsPerDay <- aggregate(steps ~ date, fullData, FUN = sum)
> g <- ggplot (stepsPerDay, aes (x = steps))
> g + geom_histogram(fill = "yellow", binwidth = 1000) +
+     labs(title = " Histogram of Steps Taken Each Day ", x = "Steps", y = "Frequency")

> stepsMean <- mean(stepsPerDay$steps, na.rm=TRUE)
> stepsMean
[1] 10766.19
> stepsMedian <- median(stepsPerDay$steps, na.rm=TRUE)
> stepsMedian
[1] 10765



> stepsPerInterval <- aggregate(steps ~ interval, fullData, mean)
> h <- ggplot (stepsPerInterval, aes(x=interval, y=steps))
> h + geom_line()+ labs(title = " Time Series Plot of Average Steps per Interval", x = "Interval", y = "Average Steps across All Days")
> maxInterval <- stepsPerInterval[which.max(stepsPerInterval$steps), ] 
> maxInterval
    interval    steps
104      835 206.1698.
> noMissingValue <- nrow(fullData[is.na(fullData$steps),])
> noMissingValue
[1] 2304



> header=TRUE
> fullData$day <- weekdays(as.Date(fullData$date))
> stepsAvg <- aggregate(steps ~ interval + day, fullData, mean)
> nadata <- fullData [is.na(fullData$steps),]
> newdata1 <- merge(nadata, stepsAvg, by=c("interval", "day"))
> View(newdata1)



> cleanData <- fullData [!is.na(fullData$steps),]
> newdata2 <- newdata1[,c(5,4,1,2)]
> colnames(newdata2) <- c("steps", "date", "interval", "day")
> mergeData <- rbind (cleanData, newdata2)



> stepsPerDayFill <- aggregate(steps ~ date, mergeData, FUN = sum)
> g1 <- ggplot (stepsPerDayFill, aes (x = steps))
> g1 + geom_histogram(fill = "green", binwidth = 1000) +
+     labs(title = " Histogram of Steps Taken Each Day ", x = "Steps", y = "Frequency")
> stepsMeanFill <- mean(stepsPerDayFill$steps, na.rm=TRUE)
> stepsMeanFill
[1] 10821.21
> stepsMedianFill <- median(stepsPerDayFill$steps, na.rm=TRUE)
> stepsMedianFill
[1] 11015


> mergeData$DayType <- ifelse(mergeData$day %in% c("Saturday", "Sunday"), "Weekend", "Weekday")
> stepsPerIntervalDT <- aggregate(steps ~ interval+DayType, mergeData, FUN = mean)
> j <- ggplot (stepsPerIntervalDT, aes(x=interval, y=steps))
> j + geom_line()+ labs(title = " Time Series Plot of Average Steps per Interval: weekdays vs. weekends", x = "Interval", y = "Average Number of Steps") + facet_grid(DayType ~ .)

