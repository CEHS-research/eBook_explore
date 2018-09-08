# Example: Ihno's Dataset



![](images/common/Ihno_header.PNG)




```r
library(tidyverse)       # super helpful everything!
library(haven)           # inporting SPSS, SAS, & Stata data files
library(furniture)       # nice tables of descriptives
```



## Background of Data

<div class="rmdlink">
<p><span class="citation">@epse4</span>: has made the data from his textbook &quot;Explaining Spychological Statistics, 4th edition&quot; available on his <a href="http://www.psych.nyu.edu/cohen/EPS4e.html">website</a></p>
</div>


 The data come from a *hypothetical* study performed by **Ihno** *(pronounced “Eee-know”)*, an advanced doctoral student, who was the teaching assistant (TA) for several sections of a statistics course. The **100 participants** in the data set are the students who were enrolled in Ihno’s sections, and voluntarily consented to be in her study, which was approved by the appropriate review board at her hypothetical school. Her data were collected on two different days. On the ﬁrst day of classes, the students who came to one of Ihno’s sections ﬁlled in a brief **background questionnaire** on which they provided contact information, some qualitative data (`gender`, undergrad `major`, why they had enrolled in statistics (`reason`), and whether they have a habit of drinking `coffee`), and some quantitative data (number of math courses already completed (`prevmath`), the score they received on a diagnostic math background quiz they were all required to take before registering for statistics `mathquiz`, and a rating of their math `phobia` on a scale from 0 to 10). You will see that, due to late registration and other factors, not all of Ihno’s students took the diagnostic math background quiz.

 The rest of Ihno’s data were collected as part of an **experiment** that she conducted during her recitation sessions on one day in the middle of the semester. (The one exception is that her students took a regular 10 question quiz the week before her experiment (`statquiz`), and she decided to add those scores to her data set.) At the beginning of the experiment, Ihno explained how each student could take his or her own pulse. She then provided a half-minute interval during which they counted the number of beats, and then wrote down twice that number as their heart rate (`hr_base`) in beats per minute (bpm). Then, each student reported how many cups of coffee they had consumed since waking up that morning (`num_cups`), and ﬁlled out an anxiety questionnaire consisting of 10 items, each rated (0 to 4) on a 5-point Likertscale. Total scores could range from 0 to 40, and provided a measure of baseline anxiety (`anx_base`). 

 Next, Ihno announced a pop quiz. She handed out a page containing 11 multiple-choice statistics questions on material covered during the preceding two weeks, and asked the students to keep this page face down while taking and recording their  pulse (`hr_pre`) and ﬁlling out a anxiety questionnaire (`anx_pre`). Then Ihno told the students they had 15 minutes to take the fairly difﬁcult quiz. She also told them that the ﬁrst 10 questions were worth 1 point each but that the 11th question was worth 3 points of extra credit. Ihno’s experimental manipulation consisted of varying the difﬁculty of the 11th question. Twenty-ﬁve quizzes were distributed at each level of difﬁculty of the ﬁnal question: easy, moderate, difﬁcult, and impossible to solve (`exp_cond`). After the quizzes were collected, Ihno asked the students to provide heart rate and anxiety data one more time (`hr_post`, `anx_post`). Finally, Ihno explained the experiment, adding that the 11th quiz question would not be scored and that, although the students would get back their quizzes with their score for the ﬁrst 10 items (`statquiz`), that score would not inﬂuence their grade for the statistics course.
 
 
 <div class="rmdlightbulb">
 <p>You can use a file's <strong>link</strong> to read data directly off a website!</p>
 </div>



```r
data_ihno <- read_spss("http://www.psych.nyu.edu/cohen/Ihno_dataset.sav") %>% 
  dplyr::rename_all(tolower) %>% 
  dplyr::mutate(gender = factor(gender, 
                                levels = c(1, 2),
                                labels = c("Female", 
                                          "Male"))) %>% 
  dplyr::mutate(major = factor(major, 
                               levels = c(1, 2, 3, 4,5),
                               labels = c("Psychology",
                                         "Premed",
                                         "Biology",
                                         "Sociology",
                                         "Economics"))) %>% 
  dplyr::mutate(reason = factor(reason,
                                levels = c(1, 2, 3),
                                labels = c("Program requirement",
                                           "Personal interest",
                                           "Advisor recommendation"))) %>% 
  dplyr::mutate(exp_cond = factor(exp_cond,
                                  levels = c(1, 2, 3, 4),
                                  labels = c("Easy",
                                             "Moderate",
                                             "Difficult",
                                             "Impossible"))) %>% 
  dplyr::mutate(coffee = factor(coffee,
                                levels = c(0, 1),
                                labels = c("Not a regular coffee drinker",
                                           "Regularly drinks coffee"))) 
```




```r
glimpse(data_ihno)
```

```
Observations: 100
Variables: 18
$ sub_num  <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16...
$ gender   <fct> Female, Female, Female, Female, Female, Female, Femal...
$ major    <fct> Psychology, Psychology, Psychology, Psychology, Psych...
$ reason   <fct> Advisor recommendation, Personal interest, Program re...
$ exp_cond <fct> Easy, Easy, Easy, Easy, Easy, Moderate, Moderate, Mod...
$ coffee   <fct> Regularly drinks coffee, Not a regular coffee drinker...
$ num_cups <dbl> 0, 0, 0, 0, 1, 1, 0, 2, 0, 2, 1, 0, 1, 2, 3, 0, 0, 3,...
$ phobia   <dbl> 1, 1, 4, 4, 10, 4, 4, 4, 4, 5, 5, 4, 7, 4, 3, 8, 4, 5...
$ prevmath <dbl> 3, 4, 1, 0, 1, 1, 2, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1,...
$ mathquiz <dbl> 43, 49, 26, 29, 31, 20, 13, 23, 38, NA, 29, 32, 18, N...
$ statquiz <dbl> 6, 9, 8, 7, 6, 7, 3, 7, 8, 7, 8, 8, 1, 5, 8, 3, 8, 7,...
$ exp_sqz  <dbl> 7, 11, 8, 8, 6, 6, 4, 7, 7, 6, 10, 7, 3, 4, 6, 1, 7, ...
$ hr_base  <dbl> 71, 73, 69, 72, 71, 70, 71, 77, 73, 78, 74, 73, 73, 7...
$ hr_pre   <dbl> 68, 75, 76, 73, 83, 71, 70, 87, 72, 76, 72, 74, 76, 8...
$ hr_post  <dbl> 65, 68, 72, 78, 74, 76, 66, 84, 67, 74, 73, 74, 78, 7...
$ anx_base <dbl> 17, 17, 19, 19, 26, 12, 12, 17, 20, 20, 21, 32, 19, 1...
$ anx_pre  <dbl> 22, 19, 14, 13, 30, 15, 16, 19, 14, 24, 25, 35, 23, 2...
$ anx_post <dbl> 20, 16, 15, 16, 25, 19, 17, 22, 17, 19, 22, 33, 20, 2...
```


## Tabulate Summary Statistics


### Specify a few variables


```r
data_ihno %>% 
  furniture::table1(phobia, statquiz, mathquiz) 
```

```

----------------------------
          Mean/Count (SD/%)
          n = 100          
 phobia                    
          3.3 (2.4)        
 statquiz                  
          6.9 (1.7)        
 mathquiz                  
          29.1 (9.5)       
----------------------------
```

### Seperate Columns BY a categorical factor


```r
data_ihno %>% 
  dplyr::mutate(phobia_cut3 = cut(phobia, 
                                  breaks = c(0, 2, 4, 10),
                                  include.lowest = TRUE)) %>% 
  furniture::table1(reason, coffee, statquiz, mathquiz,
                    splitby = ~ phobia_cut3,
                    test = TRUE)
```

```

--------------------------------------------------------------------------
                                              phobia_cut3 
                                 [0,2]      (2,4]      (4,10]     P-Value
                                 n = 39     n = 37     n = 24            
 reason                                                           <.001  
    Program requirement          4 (10.3%)  23 (62.2%) 14 (58.3%)        
    Personal interest            25 (64.1%) 3 (8.1%)   1 (4.2%)          
    Advisor recommendation       10 (25.6%) 11 (29.7%) 9 (37.5%)         
 coffee                                                           0.364  
    Not a regular coffee drinker 26 (66.7%) 19 (51.4%) 13 (54.2%)        
    Regularly drinks coffee      13 (33.3%) 18 (48.6%) 11 (45.8%)        
 statquiz                                                         0.001  
                                 7.6 (1.3)  6.6 (1.6)  6.1 (2.0)         
 mathquiz                                                         0.014  
                                 32.6 (8.5) 26.5 (9.8) 26.8 (8.9)        
--------------------------------------------------------------------------
```





## Plot of Raw Data

### Histogram: for a Single continuous variable


```r
data_ihno %>% 
  ggplot() +
  aes(phobia) +
  geom_histogram(binwidth = 1)
```

<img src="70-exmple_ihno_files/figure-html/unnamed-chunk-9-1.png" width="576" style="display: block; margin: auto;" />



```r
data_ihno %>% 
  dplyr::mutate(phobia_cut3 = cut(phobia, 
                                  breaks = c(0, 2, 4, 10),
                                  include.lowest = TRUE)) %>% 
  ggplot() +
  aes(phobia,
      fill = phobia_cut3) +
  geom_histogram(binwidth = 1)
```

<img src="70-exmple_ihno_files/figure-html/unnamed-chunk-10-1.png" width="576" style="display: block; margin: auto;" />


### Scatterplot: for two continuous variables


```r
data_ihno %>% 
  ggplot() +
  aes(x = mathquiz,
      y = statquiz) +
  geom_point()
```

<img src="70-exmple_ihno_files/figure-html/unnamed-chunk-11-1.png" width="576" style="display: block; margin: auto;" />


```r
data_ihno %>% 
  dplyr::mutate(phobia_cut3 = cut(phobia, 
                                  breaks = c(0, 2, 4, 10),
                                  include.lowest = TRUE)) %>% 
  ggplot() +
  aes(x = mathquiz,
      y = statquiz,
      color = phobia_cut3) +
  geom_point()
```

<img src="70-exmple_ihno_files/figure-html/unnamed-chunk-12-1.png" width="576" style="display: block; margin: auto;" />



```r
data_ihno %>% 
  dplyr::mutate(phobia_cut3 = cut(phobia, 
                                  breaks = c(0, 2, 4, 10),
                                  include.lowest = TRUE)) %>% 
  ggplot() +
  aes(x = mathquiz,
      y = statquiz) +
  geom_count() +
  facet_grid(. ~ phobia_cut3)
```

<img src="70-exmple_ihno_files/figure-html/unnamed-chunk-13-1.png" width="576" style="display: block; margin: auto;" />






