# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data



```r
activity <- read.csv("activity.csv")
```


## What is mean total number of steps taken per day?

```r
mydata <- na.omit(activity)
aggdata1 <- aggregate(mydata$steps, list(date = mydata$date), sum)
barplot(aggdata1$x, xlab = "date", ylab = " total number of steps taken each day", 
    names.arg = aggdata1$date)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

```r
agg_mean <- aggregate(mydata$steps, list(date = mydata$date), mean)
agg_median <- aggregate(mydata$steps, list(date = mydata$date), median)
```


## What is the average daily activity pattern?

```r
average_steps <- aggregate(mydata$steps, list(interval = mydata$interval), mean)
plot(average_steps$interval, average_steps$x, type = "l", xlab = "5-minute interval", 
    ylab = "average steps")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

```r
# order by average steps, 835 contains the maximum number of steps
neworder <- average_steps[order(x), ]
```

```
## Error: object 'x' not found
```


## Imputing missing values

```r
total_NA <- sum(is.na(activity$steps))
total_NA
```

```
## [1] 2304
```

```r
for (i in 1:2304) {
    x <- activity[is.na(activity), ]
    if (x[i, 3] == average_steps$interval) {
        activity[is.na(activity), ][i, 1] <- average_steps$x
    }
}
```

```
## Warning: the condition has length > 1 and only the first element will be
## used
```

```
## Error: replacement has 288 rows, data has 1
```

## Are there differences in activity patterns between weekdays and weekends?
