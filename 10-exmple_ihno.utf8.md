# Introduce: Ihno's Experiemnt



![](images/common/Ihno_header.PNG)




```r
library(tidyverse)       # super helpful everything!
library(haven)           # inporting SPSS, SAS, & Stata data files
```



## Background of Data

<div class="rmdlink">
<p><span class="citation">@epse4</span>: has made the data from his textbook “Explaining Spychological Statistics, 4th edition” available on his <a href="http://www.psych.nyu.edu/cohen/EPS4e.html">website</a></p>
</div>


 The data come from a *hypothetical* study performed by **Ihno** *(pronounced “Eee-know”)*, an advanced doctoral student, who was the teaching assistant (TA) for several sections of a statistics course. The **100 participants** in the data set are the students who were enrolled in Ihno’s sections, and voluntarily consented to be in her study, which was approved by the appropriate review board at her hypothetical school. Her data were collected on two different days. On the ﬁrst day of classes, the students who came to one of Ihno’s sections ﬁlled in a brief **background questionnaire** on which they provided contact information, some qualitative data (`gender`, undergrad `major`, why they had enrolled in statistics (`reason`), and whether they have a habit of drinking `coffee`), and some quantitative data (number of math courses already completed (`prevmath`), the score they received on a diagnostic math background quiz they were all required to take before registering for statistics `mathquiz`, and a rating of their math `phobia` on a scale from 0 to 10). You will see that, due to late registration and other factors, not all of Ihno’s students took the diagnostic math background quiz.

 The rest of Ihno’s data were collected as part of an **experiment** that she conducted during her recitation sessions on one day in the middle of the semester. (The one exception is that her students took a regular 10 question quiz the week before her experiment (`statquiz`), and she decided to add those scores to her data set.) At the beginning of the experiment, Ihno explained how each student could take his or her own pulse. She then provided a half-minute interval during which they counted the number of beats, and then wrote down twice that number as their heart rate (`hr_base`) in beats per minute (bpm). Then, each student reported how many cups of coffee they had consumed since waking up that morning (`num_cups`), and ﬁlled out an anxiety questionnaire consisting of 10 items, each rated (0 to 4) on a 5-point Likertscale. Total scores could range from 0 to 40, and provided a measure of baseline anxiety (`anx_base`). 

 Next, Ihno announced a pop quiz. She handed out a page containing 11 multiple-choice statistics questions on material covered during the preceding two weeks, and asked the students to keep this page face down while taking and recording their  pulse (`hr_pre`) and ﬁlling out a anxiety questionnaire (`anx_pre`). Then Ihno told the students they had 15 minutes to take the fairly difﬁcult quiz. She also told them that the ﬁrst 10 questions were worth 1 point each but that the 11th question was worth 3 points of extra credit. Ihno’s experimental manipulation consisted of varying the difﬁculty of the 11th question. Twenty-ﬁve quizzes were distributed at each level of difﬁculty of the ﬁnal question: easy, moderate, difﬁcult, and impossible to solve (`exp_cond`). After the quizzes were collected, Ihno asked the students to provide heart rate and anxiety data one more time (`hr_post`, `anx_post`). Finally, Ihno explained the experiment, adding that the 11th quiz question would not be scored and that, although the students would get back their quizzes with their score for the ﬁrst 10 items (`statquiz`), that score would not inﬂuence their grade for the statistics course.
 
 
<div class="rmdlightbulb">
<p>You can use a file’s <strong>link</strong> to read data directly off a website</p>
</div>



```r
data_ihno <- read_spss("http://www.psych.nyu.edu/cohen/Ihno_dataset.sav") %>% 
  dplyr::rename_all(tolower) %>% 
  dplyr::mutate(genderF = factor(gender, 
                                 levels = c(1, 2),
                                 labels = c("Female", 
                                            "Male"))) %>% 
  dplyr::mutate(majorF = factor(major, 
                                levels = c(1, 2, 3, 4,5),
                                labels = c("Psychology",
                                           "Premed",
                                           "Biology",
                                           "Sociology",
                                           "Economics"))) %>% 
  dplyr::mutate(reasonF = factor(reason,
                                 levels = c(1, 2, 3),
                                 labels = c("Program requirement",
                                            "Personal interest",
                                            "Advisor recommendation"))) %>% 
  dplyr::mutate(exp_condF = factor(exp_cond,
                                   levels = c(1, 2, 3, 4),
                                   labels = c("Easy",
                                              "Moderate",
                                              "Difficult",
                                              "Impossible"))) %>% 
  dplyr::mutate(coffeeF = factor(coffee,
                                 levels = c(0, 1),
                                 labels = c("Not a regular coffee drinker",
                                            "Regularly drinks coffee"))) 
```




```r
glimpse(data_ihno)
```

```
Observations: 100
Variables: 23
$ sub_num   <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 1...
$ gender    <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1...
$ major     <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1...
$ reason    <dbl> 3, 2, 1, 1, 1, 1, 1, 3, 1, 1, 1, 1, 1, 3, 1, 1, 1, 1...
$ exp_cond  <dbl> 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 3, 4, 4, 4, 4...
$ coffee    <dbl> 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1...
$ num_cups  <dbl> 0, 0, 0, 0, 1, 1, 0, 2, 0, 2, 1, 0, 1, 2, 3, 0, 0, 3...
$ phobia    <dbl> 1, 1, 4, 4, 10, 4, 4, 4, 4, 5, 5, 4, 7, 4, 3, 8, 4, ...
$ prevmath  <dbl> 3, 4, 1, 0, 1, 1, 2, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1...
$ mathquiz  <dbl> 43, 49, 26, 29, 31, 20, 13, 23, 38, NA, 29, 32, 18, ...
$ statquiz  <dbl> 6, 9, 8, 7, 6, 7, 3, 7, 8, 7, 8, 8, 1, 5, 8, 3, 8, 7...
$ exp_sqz   <dbl> 7, 11, 8, 8, 6, 6, 4, 7, 7, 6, 10, 7, 3, 4, 6, 1, 7,...
$ hr_base   <dbl> 71, 73, 69, 72, 71, 70, 71, 77, 73, 78, 74, 73, 73, ...
$ hr_pre    <dbl> 68, 75, 76, 73, 83, 71, 70, 87, 72, 76, 72, 74, 76, ...
$ hr_post   <dbl> 65, 68, 72, 78, 74, 76, 66, 84, 67, 74, 73, 74, 78, ...
$ anx_base  <dbl> 17, 17, 19, 19, 26, 12, 12, 17, 20, 20, 21, 32, 19, ...
$ anx_pre   <dbl> 22, 19, 14, 13, 30, 15, 16, 19, 14, 24, 25, 35, 23, ...
$ anx_post  <dbl> 20, 16, 15, 16, 25, 19, 17, 22, 17, 19, 22, 33, 20, ...
$ genderF   <fct> Female, Female, Female, Female, Female, Female, Fema...
$ majorF    <fct> Psychology, Psychology, Psychology, Psychology, Psyc...
$ reasonF   <fct> Advisor recommendation, Personal interest, Program r...
$ exp_condF <fct> Easy, Easy, Easy, Easy, Easy, Moderate, Moderate, Mo...
$ coffeeF   <fct> Regularly drinks coffee, Not a regular coffee drinke...
```



