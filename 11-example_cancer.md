# Introduce: Cancer Experiemnt


```r
library(tidyverse)       # super helpful everything!
library(haven)           # inporting SPSS, SAS, & Stata data files
```



## Source of Data 

Mid-Michigan Medical Center, Midland, Michigan, 1999: A  study of oral condition of cancer patients.

## Description of the Study 

The data set contains part of the data for a study of **oral condition** of cancer patients conducted at the Mid-Michigan Medical Center.  The oral conditions of the patients were measured and recorded at the *initial* stage, at the end of the *second week*, at the end of the *fourth week*, and at the end of the *sixth week*.  The variables age, initial weight and initial cancer stage of the patients were recorded.  Patients were divided into two groups **at random**:  One group received a placebo and the other group received aloe juice treatment.


> Sample size n = 25 patients with neck cancer. 
>
> The treatment is Aloe Juice. 

## Variables 

* `ID` patient identification number

* `trt` treatment group 
    + `0` *placebo* 
    + `1` *aloe juice*

* `age` patient's age, *in years*

* `weightin` patient's weight *(pounds)* at the initial stage

* `stage`	initial cancer stage
    + coded `1` through `4`

* `totalcin` oral condition at the *initial stage*
* `totalcw2` oral condition at the end of *week 2*
* `totalcw4` oral condition at the end of *week 4*
* `totalcw6` oral condition at the end of *week 6*





## Import Data

The `Cancer` dataset is saved in SPSS format, which is evident from the `.sav` ending on the file name.

The `haven` package is downloaded as part of the `tidyverse` set of packages, but is not automatically loaded.  It must have its own `library()` function call *(see above)*.  The `haven::read_spss()` function works very simarly to the `readxl::read_excel()` function we used last chapter [@R-haven].

* Make sure the **dataset** is saved in the same *folder* as this file
* Make sure the that *folder* is the **working directory**


```r
cancer_raw <- haven::read_spss("https://github.com/CEHS-research/PSY-6600_students/raw/master/Data/Cancer.sav")
```



```r
tibble::glimpse(cancer_raw)
```

```
## Observations: 25
## Variables: 9
## $ ID       <dbl> 1, 5, 6, 9, 11, 15, 21, 26, 31, 35, 39, 41, 45, 2, 12...
## $ TRT      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1,...
## $ AGE      <dbl> 52, 77, 60, 61, 59, 69, 67, 56, 61, 51, 46, 65, 67, 4...
## $ WEIGHIN  <dbl> 124.0, 160.0, 136.5, 179.6, 175.8, 167.6, 186.0, 158....
## $ STAGE    <dbl> 2, 1, 4, 1, 2, 1, 1, 3, 1, 1, 4, 1, 1, 2, 4, 1, 2, 1,...
## $ TOTALCIN <dbl> 6, 9, 7, 6, 6, 6, 6, 6, 6, 6, 7, 6, 8, 7, 6, 4, 6, 6,...
## $ TOTALCW2 <dbl> 6, 6, 9, 7, 7, 6, 11, 11, 9, 4, 8, 6, 8, 16, 10, 6, 1...
## $ TOTALCW4 <dbl> 6, 10, 17, 9, 16, 6, 11, 15, 6, 8, 11, 9, 9, 9, 11, 8...
## $ TOTALCW6 <dbl> 7, 9, 19, 3, 13, 11, 10, 15, 8, 7, 11, 6, 10, 10, 9, ...
```


## Wrangle Data


```r
cancer_clean <- cancer_raw %>% 
  dplyr::rename_all(tolower) %>% 
  dplyr::mutate(id = factor(id)) %>% 
  dplyr::mutate(trt = factor(trt,
                             labels = c("Placebo", 
                                        "Aloe Juice"))) %>% 
  dplyr::mutate(stage = factor(stage))
```




```r
tibble::glimpse(cancer_clean)
```

```
## Observations: 25
## Variables: 9
## $ id       <fct> 1, 5, 6, 9, 11, 15, 21, 26, 31, 35, 39, 41, 45, 2, 12...
## $ trt      <fct> Placebo, Placebo, Placebo, Placebo, Placebo, Placebo,...
## $ age      <dbl> 52, 77, 60, 61, 59, 69, 67, 56, 61, 51, 46, 65, 67, 4...
## $ weighin  <dbl> 124.0, 160.0, 136.5, 179.6, 175.8, 167.6, 186.0, 158....
## $ stage    <fct> 2, 1, 4, 1, 2, 1, 1, 3, 1, 1, 4, 1, 1, 2, 4, 1, 2, 1,...
## $ totalcin <dbl> 6, 9, 7, 6, 6, 6, 6, 6, 6, 6, 7, 6, 8, 7, 6, 4, 6, 6,...
## $ totalcw2 <dbl> 6, 6, 9, 7, 7, 6, 11, 11, 9, 4, 8, 6, 8, 16, 10, 6, 1...
## $ totalcw4 <dbl> 6, 10, 17, 9, 16, 6, 11, 15, 6, 8, 11, 9, 9, 9, 11, 8...
## $ totalcw6 <dbl> 7, 9, 19, 3, 13, 11, 10, 15, 8, 7, 11, 6, 10, 10, 9, ...
```



```r
cancer_clean
```

```
## # A tibble: 25 x 9
##    id    trt       age weighin stage totalcin totalcw2 totalcw4 totalcw6
##    <fct> <fct>   <dbl>   <dbl> <fct>    <dbl>    <dbl>    <dbl>    <dbl>
##  1 1     Placebo    52    124  2            6        6        6        7
##  2 5     Placebo    77    160  1            9        6       10        9
##  3 6     Placebo    60    136. 4            7        9       17       19
##  4 9     Placebo    61    180. 1            6        7        9        3
##  5 11    Placebo    59    176. 2            6        7       16       13
##  6 15    Placebo    69    168. 1            6        6        6       11
##  7 21    Placebo    67    186  1            6       11       11       10
##  8 26    Placebo    56    158  3            6       11       15       15
##  9 31    Placebo    61    213. 1            6        9        6        8
## 10 35    Placebo    51    189  1            6        4        8        7
## # ... with 15 more rows
```

