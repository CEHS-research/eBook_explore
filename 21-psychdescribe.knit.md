# Detailed Summary Statistics

Using the `psych::describe()` function



```r
library(psych)           # Lots of good tidbits
```


The `describe()` function from the `psych` package returns an extensive listing of **basic summary statistics** for every variable in a dataset [@R-psych]. 

* `vars` number order of the variables in this table
* `n` how many non-missing values there are
* `mean` the average or arithmetic mean
* `sd` the standard deviation 
* `median` the 50th percentile or Q2
* `trimmed` the mean after removing the top and bottom 10% of values
* `mad` median absolute deviation (from the median) DO NOT WORRY ABOUT!
* `min` the minimum or lowest value
* `max` the maximum or highest value
* `range` full range of values, max - min
* `skew` skewness (no SE for skewness given)
* `kurtosis` kurtosis (no SE for kurtosis given)
* `se` the standard error for the **MEAN**, not the skewness or kurtosis


## All Variables in a Dataset






