# Reproducible Research: Peer Assessment 1

 
## Loading and preprocessing the data
Extract the activity.cvs file from activity.zip,  
created data.frame from the csv file and wrap the data frame into a data table

```r
library(data.table)
library(dplyr)
activity <- data.table(read.csv(unz("activity.zip","activity.csv"))) %>%
            mutate(fulldate = strptime(date, format="%Y-%m-%d") + (interval %/% 100) * 60 + interval %% 100)  
```

## What is mean total number of steps taken per day?
Group the data by day, and summerize the steps by date. 
For dates that do not have steps, the number of steps is set to zero

```r
library(xtable)
number.of.steps.by.day <- activity %>% 
                          group_by(date) %>% 
                          summarise_each(funs(sum(., na.rm = TRUE)), steps)
```

In the historgram the number of steps per day are show  

```r
barplot( number.of.steps.by.day$steps, names.arg=number.of.steps.by.day$date, 
         ylab="Number of steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 


```r
mean.steps <- mean(number.of.steps.by.day$steps)
median.steps <- median(number.of.steps.by.day$steps)
```
The mean number of step per day is 9354.2295082 steps per day.  
The median is 10395 steps per day.


## What is the average daily activity pattern?

```r
library(ggplot2)
library(scales)
mean.steps <- mean(activity$steps, na.rm = TRUE)
##plot(activity$fulldate, activity$steps, type="l", xlab="", ylab="Number of steps per 5 minut interval")
##lines(activity$date, activity$steps)
##abline(h=mean.steps, col="red")
p <- qplot(activity$fulldate, activity$steps, geom="line") +scale_x_datetime(breaks = date_breaks("10 days"))
p <- p + geom_abline(intercept = mean.steps, slope = 0, colour ="red")
print(p)
```

```
## Warning in loop_apply(n, do.ply): Removed 576 rows containing missing
## values (geom_path).
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?









To create a histogram on a day basis, the precentage steps per day relative to the total number of steps has to be calculated, 

```r
# total.number.of.steps <-sum(activity$steps, na.rm = TRUE)
# number.of.steps.by.day <- mutate(number.of.steps.by.day, percentage = steps / total.number.of.steps * 100)
```



