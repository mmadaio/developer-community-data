Online Developer Community Survey Analyses
================

-   [Overview](#overview)
    -   [Research questions](#research-questions)
-   [Setup](#setup)
    -   [Load Data](#load-data)
        -   [Define data folder](#define-data-folder)
        -   [Load Stack Overflow](#load-stack-overflow)
        -   [Load GitHub](#load-github)
        -   [Load HackerRank](#load-hackerrank)
    -   [Clean Data](#clean-data)
        -   [Stack Overflow](#stack-overflow)
        -   [GitHub](#github)
        -   [HackerRank](#hackerrank)
-   [Chi-Square analyses](#chi-square-analyses)
    -   [RQ1: Relationship between Gender and Participation in Online Developer Communities](#rq1-relationship-between-gender-and-participation-in-online-developer-communities)
        -   [Registered Account](#registered-account)
        -   [Visit Frequency](#visit-frequency)
        -   [Participation - Frequency](#participation---frequency)
        -   [Participation - Contribute](#participation---contribute)
        -   [Participation - Follow](#participation---follow)
        -   [Contribution - Code](#contribution---code)
        -   [Contribution - Interest](#contribution---interest)
        -   [Contribution - Likelihood](#contribution---likelihood)
    -   [RQ2: Relationship between Gender and Perception of Online Developer Communities](#rq2-relationship-between-gender-and-perception-of-online-developer-communities)
        -   [Welcoming Community](#welcoming-community)
        -   [Code of Conduct](#code-of-conduct)
        -   [Unwelcoming Language](#unwelcoming-language)
        -   [Considered Member (Stack Overflow)](#considered-member-stack-overflow)
        -   [Consider Member (GitHub)](#consider-member-github)
        -   [Kinship](#kinship)
        -   [Kinship](#kinship-1)
        -   [Self-Efficacy (Stack Overflow)](#self-efficacy-stack-overflow)
        -   [Self-Efficacy (GitHub)](#self-efficacy-github)
        -   [RQ3: Relationship between Gender and Employment / Years of experience coding](#rq3-relationship-between-gender-and-employment-years-of-experience-coding)
    -   [Post-Hoc Correction for Multiple Comparisons](#post-hoc-correction-for-multiple-comparisons)

Overview
========

Online developer communities boast millions of users - over 29 million on GitHub and over 8 million on Stack Overflow, in 2018. Participation in these communities is becoming one of the primary ways software developers learn new programming languages, improve their skills, develop collaborative projects, and find new job opportunities. [\[1\]](David%20and%20Shapiro,%202008) [\[2\]](Ford%20et%20al.,%202016) [\[3\]](Vasilescu%20et%20al.,%202015)

Developers on these sites may ask and answer coding questions to improve their skills (e.g. Stack Overflow), use those skills to contribute to open-source code (e.g. GitHub) and participate in coding challenges (e.g. HackerRank). These platforms are becoming increasingly important to hiring decisions, as recruiters look at GitHub contributions or reputation on Stack Overflow as indicators of developers' skill.

However, despite the promise for these online developer communities to support software developers in their professional development, there are indicators that there may be serious difference in women and men's participation in these communities - differences which may further exacerbate existing gender gaps in the ICT workforce writ large.

Research questions
------------------

To understand the extent and nature of the gender gap in online developer communities, we ask the following research questions:

1.  How do male and female developers differ in their participation in online developer communities?
2.  How do male and female developers differ in their perceptions of belonging and kinship in online developer communities?
3.  How do male and female developers in online communities differ in their employment and prior experience with coding?

Setup
=====

These lines install the necessary packages and libraries needed to run the code. The packages only need to be installed the first time you run it.

``` r
#install.packages("survey")
#install.packages("vcd")
path_to_file = "C:/Users/michael.madaio/Downloads/fifer_1.1.tar.gz"
#install.packages(c('party', 'plotrix', 'randomForest', 'randomForestSRC', 'Hmisc', 'fields'))
#install.packages(path_to_file, repos = NULL, type="source")
#install.packages("COUNT")
#install.packages("reticulate")
#install.packages("rmarkdown")

library(COUNT)
library("survey")
library("MASS")
library(vcd)
library("fifer")
library(reticulate)
```

Load Data
---------

Load in the .csv data sets. We use surveys from 3 major online developer communities:

-   Stack Overflow [survey](https://insights.stackoverflow.com/survey/2018/) (download latest survey results [here](https://drive.google.com/uc?export=download&id=1_9On2-nsBQIw3JiY43sWbrF8EjrqrR4U))
-   GitHub [survey](http://opensourcesurvey.org/2017/) (download latest survey results [here](https://github.com/github/open-source-survey/releases/download/v1.0/data_for_public_release.zip))
-   HackerRank [survey](https://www.kaggle.com/hackerrank/developer-survey-2018/home) (download latest survey results [here](https://www.kaggle.com/hackerrank/developer-survey-2018/))

### Define data folder

``` r
data_folder = "C:/Users/michael.madaio/Documents/EQUALS/Case study - Developer communities/Data/"
```

### Load Stack Overflow

``` r
so_file = "survey_results_public.csv"
so_path = paste(data_folder, so_file, sep = "", collapse = NULL)
so_df <- read.csv(so_path, header=T, stringsAsFactors=TRUE) 
```

### Load GitHub

``` r
gh_file = "survey_data.csv"
gh_path = paste(data_folder, gh_file, sep = "", collapse = NULL)
gh_df <- read.csv(gh_path, header=T, stringsAsFactors=TRUE) 
```

### Load HackerRank

``` r
hr_file = "HackerRank-Developer-Survey-2018-Values.csv"
hr_path = paste(data_folder, hr_file, sep = "", collapse = NULL)
hr_df <- read.csv(hr_path, header=T, stringsAsFactors=TRUE) 
```

Clean Data
----------

Create a new dataframe for each data set that only includes respondents that indicated Male and Female on the surveys. For the purposes of this analysis, we only look for differences between respondents that selected either Male or Female. Further analyses may look for differences among non-binary, transgender, or other respondents as well.

### Stack Overflow

``` r
## Remove all rows with NA for Gender
so_mf_df <- so_df
so_mf_df <- so_mf_df[!is.na(so_mf_df$Gender),]

## Create new dataframe with respondents who selected Male or Female 
so_mf_df = so_mf_df[(so_mf_df$Gender == "Male") | (so_mf_df$Gender == "Female"), ]
so_mf_df$Gender <- so_mf_df$Gender[ , drop=TRUE]
summary(so_mf_df$Gender)
```

    ## Female   Male 
    ##   4025  59458

### GitHub

``` r
## Create new dataframe with respondents who selected Male or Female
gh_mf_df = gh_df[(gh_df$GENDER == "Man") | (gh_df$GENDER == "Woman"), ]
gh_mf_df$GENDER <- gh_mf_df$GENDER[ , drop=TRUE]
summary(gh_mf_df$GENDER)
```

    ##   Man Woman 
    ##  3387   125

### HackerRank

``` r
## Create new dataframe with respondents who selected Male or Female
hr_mf_df = hr_df[(hr_df$q3Gender == "Male") | (hr_df$q3Gender == "Female"), ]
hr_mf_df$q3Gender <- hr_mf_df$q3Gender[ , drop=TRUE]
summary(hr_mf_df$q3Gender)
```

    ## Female   Male 
    ##   4122  20774

Chi-Square analyses
===================

In this section, we test whether there is a 2-way relationship between Gender and our respective variables of interest for our research questions.

For each Chi-square test, we follow it by computing the Pearson standardized residuals (*r*<sub>*i**j*</sub>), to identify whether the observed values are significantly different from the expected values. We use an absolute value of 2 as our threshold for significance, following Agresti (2002).

As we test multiple hypotheses, we follow Chi-Squares with a post-hoc correction of the *p*-values for multiple hypothesis testing, using the Benjamini-Hochberg method (Benjamini and Hochberg, 1995). All *p*-values reported below are after adjusting the Benjamini-Hochberg correction.

------------------------------------------------------------------------

RQ1: Relationship between Gender and Participation in Online Developer Communities
----------------------------------------------------------------------------------

How do male and female developers differ in their participation in online developer communities?

We hypothesize that women will participate less overall, and in less publicly visible ways. [\[1\]](David%20and%20Shapiro,%202008) [\[2\]](Ford%20et%20al.,%202016) [\[3\]](Vasilescu%20et%20al.,%202015)

### Registered Account

**Survey Question**: "Do you have a registered account with Stack Overflow?"

There is a significant relationship between Gender and having a registered account on Stack Overflow (*p*&lt;0.001), with men more likely to have an account (*r*<sub>*i**j*</sub> = 21.4).

``` r
so_tbl_acc = table(so_mf_df$Gender, so_mf_df$StackOverflowHasAccount)
so_acc <- chisq.test(so_tbl_acc, correct=TRUE)
print(so_acc)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  so_tbl_acc
    ## X-squared = 458.38, df = 2, p-value < 2.2e-16

``` r
print(so_acc$stdres)
```

    ##         
    ##          I'm not sure / I can't remember        No       Yes
    ##   Female                        11.45502  17.40859 -21.40841
    ##   Male                         -11.45502 -17.40859  21.40841

### Visit Frequency

**Survey Question**: "How often do you visit Stack Overflow?"

There is a significant relationship between Gender and visiting Stack Overflow more often (*p*&lt;0.001), with female respondents more likely to have only visited once (*r*<sub>*i**j*</sub> = 8.98) or a few times per month (*r*<sub>*i**j*</sub> = 8.62), and men more likely to have visited multiple times per day (*r*<sub>*i**j*</sub> = 5.49).

``` r
so_tbl_visit = table(so_mf_df$Gender, so_mf_df$StackOverflowVisit)
so_visit <- chisq.test(so_tbl_visit, correct=TRUE)
print(so_visit)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  so_tbl_visit
    ## X-squared = 221.84, df = 5, p-value < 2.2e-16

``` r
print(so_visit$stdres)
```

    ##         
    ##          A few times per month or weekly A few times per week
    ##   Female                       8.6226088            0.8502722
    ##   Male                        -8.6226088           -0.8502722
    ##         
    ##          Daily or almost daily
    ##   Female            -4.1453594
    ##   Male               4.1453594
    ##         
    ##          I have never visited Stack Overflow (before today)
    ##   Female                                          8.9779666
    ##   Male                                           -8.9779666
    ##         
    ##          Less than once per month or monthly Multiple times per day
    ##   Female                           6.5897248             -5.4983760
    ##   Male                            -6.5897248              5.4983760

### Participation - Frequency

**Survey Question**: "How often do you participate in Q&A on Stack Overflow?"

There is a significant relationship between Gender and participating on Stack Overflow more often (*p*&lt;0.001), with female respondents more likely to have never participated (*r*<sub>*i**j*</sub> = 14.64), and men more likely to have participated a few times per week (*r*<sub>*i**j*</sub> = 7.53).

``` r
so_tbl_part = table(so_mf_df$Gender, so_mf_df$StackOverflowParticipate)
so_part <- chisq.test(so_tbl_part, correct=TRUE)
print(so_part)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  so_tbl_part
    ## X-squared = 315.05, df = 5, p-value < 2.2e-16

``` r
print(so_part$stdres)
```

    ##         
    ##          A few times per month or weekly A few times per week
    ##   Female                       -6.767559            -7.535377
    ##   Male                          6.767559             7.535377
    ##         
    ##          Daily or almost daily
    ##   Female             -5.267598
    ##   Male                5.267598
    ##         
    ##          I have never participated in Q&A on Stack Overflow
    ##   Female                                          14.645454
    ##   Male                                           -14.645454
    ##         
    ##          Less than once per month or monthly Multiple times per day
    ##   Female                            3.701063              -4.073401
    ##   Male                             -3.701063               4.073401

### Participation - Contribute

**Survey Question**: "Do you contribute to repositories on GitHub?"

There is a significant relationship between Gender and contributing to repositories on GitHub (*p*&lt;0.001), with male respondents more likely to have contributed (*r*<sub>*i**j*</sub> = 3.77).

``` r
gh_tbl_cont = table(gh_mf_df$GENDER, gh_mf_df$PARTICIPATION.TYPE.CONTRIBUTE)
gh_cont <- chisq.test(gh_tbl_cont, correct=TRUE)
print(gh_cont)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  gh_tbl_cont
    ## X-squared = 13.462, df = 1, p-value = 0.0002434

``` r
print(gh_cont$stdres)
```

    ##        
    ##                 0         1
    ##   Man   -3.772618  3.772618
    ##   Woman  3.772618 -3.772618

### Participation - Follow

**Survey Question**: "Do you follow other repositories on GitHub?"

There is a significant relationship between Gender and following repositories on GitHub (*p*&lt;0.001), with male respondents more likely to have followed other repositories (*r*<sub>*i**j*</sub> = 5.53).

``` r
gh_tbl_follow = table(gh_mf_df$GENDER, gh_mf_df$PARTICIPATION.TYPE.FOLLOW)
gh_follow <- chisq.test(gh_tbl_follow, correct=TRUE)
print(gh_follow)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  gh_tbl_follow
    ## X-squared = 29.229, df = 1, p-value = 6.429e-08

``` r
print(gh_follow$stdres)
```

    ##        
    ##                 0         1
    ##   Man   -5.526173  5.526173
    ##   Woman  5.526173 -5.526173

### Contribution - Code

**Survey Question**: "Do you contribute code to repositories on GitHub?"

There is a significant relationship between Gender and contributing code to repositories on GitHub (*p*&lt;0.01), with male respondents more likely to have occasionally contributed code (*r*<sub>*i**j*</sub> = 2.38).

``` r
gh_tbl_cont_code = table(gh_mf_df$GENDER, gh_mf_df$CONTRIBUTOR.TYPE.CONTRIBUTE.CODE)
gh_cont_code <- chisq.test(gh_tbl_cont_code, correct=TRUE, simulate.p.value = TRUE)
print(gh_cont_code)
```

    ## 
    ##  Pearson's Chi-squared test with simulated p-value (based on 2000
    ##  replicates)
    ## 
    ## data:  gh_tbl_cont_code
    ## X-squared = 19.835, df = NA, p-value = 0.002499

``` r
print(gh_cont_code$stdres)
```

    ##        
    ##                    Frequently      Never Occasionally     Rarely
    ##   Man   -3.7033564  0.8761179 -1.8695400    2.3863733  1.3812712
    ##   Woman  3.7033564 -0.8761179  1.8695400   -2.3863733 -1.3812712

### Contribution - Interest

**Survey Question**: "Are you interested in contributing code on GitHub in the future?"

No significant difference was observed between male and female developers' interest in contributing code on GitHub in the future.

``` r
gh_tbl_cont_int = table(gh_mf_df$GENDER, gh_mf_df$FUTURE.CONTRIBUTION.INTEREST)
gh_cont_int <- chisq.test(gh_tbl_cont_int, correct=TRUE)
```

    ## Warning in chisq.test(gh_tbl_cont_int, correct = TRUE): Chi-squared
    ## approximation may be incorrect

``` r
print(gh_cont_int)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  gh_tbl_cont_int
    ## X-squared = 6.1293, df = 4, p-value = 0.1897

### Contribution - Likelihood

**Survey Question**: "Are you likely to contribute code on GitHub in the future?"

There is a significant relationship between Gender and developers' self-reported likelihood to contribute code on GitHub in the future (*p*&lt;0.05), with male respondents more likely to say they are "very likely" to contribute code in the future (*r*<sub>*i**j*</sub> = 3.06), and female respondents more likely to say it is only "somewhat likely" they will contribute code on GitHub in the future (*r*<sub>*i**j*</sub> = 2.81).

``` r
gh_tbl_cont_like = table(gh_mf_df$GENDER, gh_mf_df$FUTURE.CONTRIBUTION.LIKELIHOOD)
gh_cont_like <- chisq.test(gh_tbl_cont_like, correct=TRUE)
```

    ## Warning in chisq.test(gh_tbl_cont_like, correct = TRUE): Chi-squared
    ## approximation may be incorrect

``` r
print(gh_cont_like)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  gh_tbl_cont_like
    ## X-squared = 11.007, df = 4, p-value = 0.02649

``` r
print(gh_cont_like$stdres)
```

    ##        
    ##                    Somewhat likely Somewhat unlikely Very likely
    ##   Man    0.1921362      -2.8082643        -0.2610031   3.0640428
    ##   Woman -0.1921362       2.8082643         0.2610031  -3.0640428
    ##        
    ##         Very unlikely
    ##   Man      -1.3063903
    ##   Woman     1.3063903

``` r
  # Man - Very likely
  # Women - Somewhat likely
```

------------------------------------------------------------------------

RQ2: Relationship between Gender and Perception of Online Developer Communities
-------------------------------------------------------------------------------

What is the relationship between developers' gender and their experience of being welcomed in online developer communities?

We hypothesize that women will be less likely to perceive themselves as members of the developer communities. [\[1\]](David%20and%20Shapiro,%202008) [\[2\]](Ford%20et%20al.,%202016) [\[3\]](Vasilescu%20et%20al.,%202015)

### Welcoming Community

**Survey Question**: "How important is a welcoming community when contributing to OS projects?"

There is a significant relationship between Gender and perceived importance of a welcoming community in open-source projects (*p*&lt;0.001), with men more likely to report that a welcoming community is only somewhat important (*r*<sub>*i**j*</sub> = 4.43).

``` r
gh_tbl_perc_comm = table(gh_mf_df$GENDER, gh_mf_df$OSS.CONTRIBUTOR.PRIORITIES.WELCOMING.COMMUNITY)
gh_comm <- chisq.test(gh_tbl_perc_comm, correct=TRUE)
```

    ## Warning in chisq.test(gh_tbl_perc_comm, correct = TRUE): Chi-squared
    ## approximation may be incorrect

``` r
print(gh_comm)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  gh_tbl_perc_comm
    ## X-squared = 30.906, df = 6, p-value = 2.642e-05

``` r
print(gh_comm$stdres)
```

    ##        
    ##                    Don't know what this is Not important either way
    ##   Man   -3.9923685               0.3844367                1.7585872
    ##   Woman  3.9923685              -0.3844367               -1.7585872
    ##        
    ##         Somewhat important not to have Somewhat important to have
    ##   Man                       -0.5182545                  4.4314968
    ##   Woman                      0.5182545                 -4.4314968
    ##        
    ##         Very important not to have Very important to have
    ##   Man                    0.6939439             -1.4636345
    ##   Woman                 -0.6939439              1.4636345

### Code of Conduct

**Survey Question**: "How important is a code of conduct when contributing to OS projects?"

There is a significant relationship between Gender and perceived importance of a code of conduct in open-source projects (*p*&lt;0.001), with women more likely to report that a code of conduct is very important (*r*<sub>*i**j*</sub> = 3.81), while men were more likely to report that it was not important either way (*r*<sub>*i**j*</sub> = 4.01).

``` r
gh_tbl_perc_code = table(gh_mf_df$GENDER, gh_mf_df$OSS.CONTRIBUTOR.PRIORITIES.CODE.OF.CONDUCT)
gh_conduct <- chisq.test(gh_tbl_perc_code, correct=TRUE)
```

    ## Warning in chisq.test(gh_tbl_perc_code, correct = TRUE): Chi-squared
    ## approximation may be incorrect

``` r
print(gh_conduct)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  gh_tbl_perc_code
    ## X-squared = 45.796, df = 6, p-value = 3.251e-08

``` r
print(gh_conduct$stdres)
```

    ##        
    ##                    Don't know what this is Not important either way
    ##   Man   -3.9671246               1.7381851                4.0132372
    ##   Woman  3.9671246              -1.7381851               -4.0132372
    ##        
    ##         Somewhat important not to have Somewhat important to have
    ##   Man                        1.8672631                  2.0518041
    ##   Woman                     -1.8672631                 -2.0518041
    ##        
    ##         Very important not to have Very important to have
    ##   Man                   -0.6686626             -3.8145679
    ##   Woman                  0.6686626              3.8145679

``` r
  # Women - Very important
  # Men - Not important eiter way, somewhat important
```

### Unwelcoming Language

**Survey Question**: "Have you experienced unwelcoming language?"

There is a significant relationship between Gender and experience of unwelcome language on GitHub (*p*&lt;0.005), with women more likely to report having experienced unwelcome language (*r*<sub>*i**j*</sub> = 3.99).

``` r
gh_tbl_perc_unwel = table(gh_mf_df$GENDER, gh_mf_df$DISCOURAGING.BEHAVIOR.UNWELCOMING.LANGUAGE)
gh_unwel <- chisq.test(gh_tbl_perc_unwel, correct=TRUE, simulate.p.value = TRUE)
print(gh_unwel)
```

    ## 
    ##  Pearson's Chi-squared test with simulated p-value (based on 2000
    ##  replicates)
    ## 
    ## data:  gh_tbl_perc_unwel
    ## X-squared = 16.732, df = NA, p-value = 0.001999

``` r
print(gh_unwel$stdres)
```

    ##        
    ##                            No        Yes
    ##   Man   -0.6916084  4.0844707 -3.9974947
    ##   Woman  0.6916084 -4.0844707  3.9974947

``` r
  # Women - Yes
  # Men - No
```

### Considered Member (Stack Overflow)

**Survey Question**: "Do you consider yourself a member of the Stack Overflow community?"

There is a significant relationship between Gender and considering oneself a member of the Stack Overflow community (*p*&lt;0.001), with men more likely to consider themselves members (*r*<sub>*i**j*</sub> = 16.92).

``` r
so_tbl_perc_member = table(so_mf_df$Gender, so_mf_df$StackOverflowConsiderMember)
so_mem <- chisq.test(so_tbl_perc_member, correct=TRUE)
print(so_mem)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  so_tbl_perc_member
    ## X-squared = 289.23, df = 2, p-value < 2.2e-16

``` r
print(so_mem$stdres)
```

    ##         
    ##          I'm not sure        No       Yes
    ##   Female      8.64075  11.61933 -16.91646
    ##   Male       -8.64075 -11.61933  16.91646

### Consider Member (GitHub)

**Survey Question**: "I consider myself to be a member of the open source (and/or the Free/Libre software) community"

There is a significant relationship between Gender and considering oneself a member of the open-source community on GitHub (*p*&lt;0.01), with men more likely to consider themselves members (*r*<sub>*i**j*</sub> = 2.55).

``` r
gh_tbl_perc_member = table(gh_mf_df$GENDER, gh_mf_df$OSS.IDENTIFICATION)
gh_mem <- chisq.test(gh_tbl_perc_member, correct=TRUE)
```

    ## Warning in chisq.test(gh_tbl_perc_member, correct = TRUE): Chi-squared
    ## approximation may be incorrect

``` r
print(gh_mem) 
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  gh_tbl_perc_member
    ## X-squared = 16.829, df = 5, p-value = 0.004837

``` r
print(gh_mem$stdres)
```

    ##        
    ##                    Neither agree nor disagree Somewhat agree
    ##   Man    0.4298746                  0.5488230     -0.7538566
    ##   Woman -0.4298746                 -0.5488230      0.7538566
    ##        
    ##         Somewhat disagree Strongly agree Strongly disagree
    ##   Man          -2.6945452      2.5468274        -2.3094580
    ##   Woman         2.6945452     -2.5468274         2.3094580

### Kinship

**Survey Question**: "I feel a sense of kinship or connection to other developers"

There is a significant relationship between Gender and perceived kinship with other developers on Stack Overflow (*p*&lt;0.05), with men more likely to strongly agree that they feel kinship (*r*<sub>*i**j*</sub> = 2.08), and women more likely to disagree (*r*<sub>*i**j*</sub> = 3.11).

``` r
so_tbl_perc_kinship = table(so_mf_df$Gender, so_mf_df$AgreeDisagree2)
so_kin <- chisq.test(so_tbl_perc_kinship, correct=TRUE)
print(so_kin)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  so_tbl_perc_kinship
    ## X-squared = 18.107, df = 4, p-value = 0.001176

``` r
print(so_kin$stdres)
```

    ##         
    ##              Agree  Disagree Neither Agree nor Disagree Strongly agree
    ##   Female -1.754715  3.115426                  -1.486336      -2.087537
    ##   Male    1.754715 -3.115426                   1.486336       2.087537
    ##         
    ##          Strongly disagree
    ##   Female          1.865483
    ##   Male           -1.865483

### Kinship

**Survey Question**: "The community values contributions from people like me"

There is a significant relationship between Gender and feeling that the GitHub community values "people like them" (*p*&lt;0.001), with women more likely to strongly disagree that the Github community values them (*r*<sub>*i**j*</sub> = 2.94).

``` r
gh_tbl_perc_value = table(gh_mf_df$GENDER, gh_mf_df$EXTERNAL.EFFICACY)
gh_val <- chisq.test(gh_tbl_perc_value, correct=TRUE)
```

    ## Warning in chisq.test(gh_tbl_perc_value, correct = TRUE): Chi-squared
    ## approximation may be incorrect

``` r
print(gh_val)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  gh_tbl_perc_value
    ## X-squared = 38.781, df = 5, p-value = 2.628e-07

``` r
print(gh_val$stdres)
```

    ##        
    ##                    Neither agree nor disagree Somewhat agree
    ##   Man    0.5087796                 -1.0385075      1.5418459
    ##   Woman -0.5087796                  1.0385075     -1.5418459
    ##        
    ##         Somewhat disagree Strongly agree Strongly disagree
    ##   Man          -5.1097529      1.9164458        -2.9368605
    ##   Woman         5.1097529     -1.9164458         2.9368605

### Self-Efficacy (Stack Overflow)

**Survey Question**: "I'm not as good at programming as my peers"

There is a significant relationship between Gender and developers' self-efficacy for programming on Stack Overflow (*p*&lt;0.001), with women more likely to strongly agree that they are not as good at programming as their peers (*r*<sub>*i**j*</sub> = 12.8), and men more likely to strongly disagree (*r*<sub>*i**j*</sub> = 12.8).

``` r
so_tbl_perc_eff = table(so_mf_df$Gender, so_mf_df$AgreeDisagree3)
so_eff <- chisq.test(so_tbl_perc_eff, correct=TRUE)
print(so_eff)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  so_tbl_perc_eff
    ## X-squared = 539.51, df = 4, p-value < 2.2e-16

``` r
print(so_eff$stdres)
```

    ##         
    ##               Agree   Disagree Neither Agree nor Disagree Strongly agree
    ##   Female  15.252245  -6.772071                   2.761076      12.800229
    ##   Male   -15.252245   6.772071                  -2.761076     -12.800229
    ##         
    ##          Strongly disagree
    ##   Female        -13.765310
    ##   Male           13.765310

### Self-Efficacy (GitHub)

**Survey Question**: "I have the skills and understanding necessary to make meaningful contributions to open source projects."

No significant difference was observed between male and female developers' self-efficacy for programming on GitHub.

``` r
gh_tbl_perc_int_eff = table(gh_mf_df$GENDER, gh_mf_df$INTERNAL.EFFICACY)
gh_int <- chisq.test(gh_tbl_perc_int_eff, correct=TRUE, simulate.p.value = TRUE)
print(gh_int)
```

    ## 
    ##  Pearson's Chi-squared test with simulated p-value (based on 2000
    ##  replicates)
    ## 
    ## data:  gh_tbl_perc_int_eff
    ## X-squared = 9.9679, df = NA, p-value = 0.1064

<!-- ### Recommend   -->
<!-- **Survey Question**: "How likely are you to recommend StackOverflow to a friend?"   -->
<!-- There is a significant relationship between Gender and  ($p$<0.05) with likelihood to recommend Stack Overflow to a friend, with men reporting being more likely to recommend Stack Overflow ($r_{ij}$ = 4.18), and women reporting being less likely to recommend it to a friend ($r_{ij}$ = 7.84).    -->
<!-- ```{r, echo=TRUE} -->
<!-- so_tbl_perc_rec = table(so_mf_df$Gender, so_mf_df$StackOverflowRecommend) -->
<!-- so_rec <- chisq.test(so_tbl_perc_rec, correct=TRUE) -->
<!-- print(so_rec) -->
<!-- print(so_rec$stdres) -->
<!-- ``` -->

------------------------------------------------------------------------

### RQ3: Relationship between Gender and Employment / Years of experience coding

<!-- #### Job Level -->
<!-- ```{r, echo=FALSE} -->
<!-- hr_tbl_job = table(hr_mf_df$q3Gender, hr_mf_df$q8JobLevel) -->
<!-- hr_job <- chisq.test(hr_tbl_job, correct=TRUE) -->
<!-- print(hr_job) -->
<!-- print(hr_job$stdres) -->
<!--   # Male - Senior developer, Architect -->
<!--   # Female - Student, Level 1 developer, New grad -->
<!-- ``` -->
<!-- #### Current Role -->
<!-- ```{r, echo=FALSE} -->
<!-- print("Current Role") -->
<!-- hr_tbl_role = table(hr_mf_df$q3Gender, hr_mf_df$q9CurrentRole) -->
<!-- hr_role <- chisq.test(hr_tbl_role, correct=TRUE) -->
<!-- print(hr_role) -->
<!-- print(hr_role$stdres) -->
<!--   # Male - Full-stack Developer, software Architect, software Engineer, Back-end Developer -->
<!--   # Female - Student, Software Test Engineer, Web Developer -->
<!-- ``` -->
<!-- #### Industry -->
<!-- ```{r, echo=FALSE} -->
<!-- hr_tbl_industry = table(hr_mf_df$q3Gender, hr_mf_df$q10Industry) -->
<!-- hr_industry <- chisq.test(hr_tbl_industry, correct=TRUE) -->
<!-- print(hr_industry) -->
<!-- print(hr_industry$stdres) -->
<!--   # Female - Education -->
<!--   # Male - Financial services, Retail -->
<!-- ``` -->
<!-- #### Hiring Manager -->
<!-- ```{r, echo=FALSE} -->
<!-- hr_tbl_hiring = table(hr_mf_df$q3Gender, hr_mf_df$q16HiringManager) -->
<!-- hr_hiring <- chisq.test(hr_tbl_hiring, correct=TRUE) -->
<!-- print(hr_hiring) -->
<!-- print(hr_hiring$stdres) -->
<!--   # Male - Yes -->
<!--   # Female - No -->
<!-- ``` -->
<!-- <!-- #### Employment Status -->
--&gt; <!-- <!-- ```{r, echo=FALSE} --> --&gt; <!-- <!-- gh_tbl_emp = table(gh_mf_df$GENDER, gh_mf_df$EMPLOYMENT.STATUS) --> --&gt; <!-- <!-- gh_emp <- chisq.test(gh_tbl_emp, correct=TRUE) --> --&gt; <!-- <!-- print(gh_emp) --> --&gt; <!-- <!-- # N.S. --> --&gt; <!-- <!-- ``` --> --&gt;

<!-- <!-- ```{r} -->
--&gt; <!-- <!-- # ### StackOverflow ### --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # ## Employment Status --> --&gt; <!-- <!-- # so_tbl_emp = table(so_mf_df$Gender, so_mf_df$Employment)  --> --&gt; <!-- <!-- # so_emp <- chisq.test(so_tbl_emp, simulate.p.value = TRUE)  --> --&gt; <!-- <!-- # print(so_emp) --> --&gt; <!-- <!-- # print(so_emp$stdres) --> --&gt; <!-- <!-- #   # Male - Freelancer or self-employed --> --&gt; <!-- <!-- #   # Female - Not employed and looking / Employed part time --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # ## Developer Type --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # so_mf_df$DevType <- as.character(sapply(so_mf_df$DevType, tolower)) --> --&gt; <!-- <!-- # print(so_mf_df$DevType) --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Aggregating from choose-all to single response. Not ideal. Loosely ordered by seniority and/or salary --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("executive", so_mf_df$DevType)] <- "Executive" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("manager", so_mf_df$DevType)] <- "Manager" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("administrator", so_mf_df$DevType)] <- "Admin" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("data scientist or machine learning specialist|data or business analyst", so_mf_df$DevType)] <- "Data Scientist" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("devops", so_mf_df$DevType)] <- "DevOps" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("full-stack", so_mf_df$DevType)] <- "Full-Stack" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("back-end", so_mf_df$DevType)] <- "Back-end" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("front-end", so_mf_df$DevType)] <- "Front-end" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("mobile", so_mf_df$DevType)] <- "Mobile" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("developer", so_mf_df$DevType)] <- "Developer" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("designer", so_mf_df$DevType)] <- "Designer" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("marketing", so_mf_df$DevType)] <- "" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("educator", so_mf_df$DevType)] <- "Educator" --> --&gt; <!-- <!-- # so_mf_df$DevType[grep("student", so_mf_df$DevType)] <- "Student" --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # so_mf_df$DevType <- factor(so_mf_df$DevType) --> --&gt; <!-- <!-- # summary(so_mf_df$DevType) --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # so_tbl_dev = table(so_mf_df$Gender, so_mf_df$DevType)  --> --&gt; <!-- <!-- # so_dev <- chisq.test(so_tbl_dev,correct=TRUE)  --> --&gt; <!-- <!-- # print(so_dev) --> --&gt; <!-- <!-- # print(so_dev$stdres) --> --&gt; <!-- <!-- #   # Women - Front-end, Data Scientist, Designer, Student, Educator  --> --&gt; <!-- <!-- #   # Men - Admin, Executive, Manager, DevOps,    --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # ``` --> --&gt; <!-- <!-- # ### Chi-Squares for Gender and Experience coding --> --&gt; <!-- <!-- # ```{r, echo=FALSE} --> --&gt; <!-- <!-- # ### Stack Overflow ### --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Years Coding --> --&gt; <!-- <!-- # so_tbl_years_coding = table(so_mf_df$Gender, so_mf_df$YearsCoding)  --> --&gt; <!-- <!-- # so_yrs_coding <- chisq.test(so_tbl_years_coding, correct=TRUE)  --> --&gt; <!-- <!-- # print(so_yrs_coding) --> --&gt; <!-- <!-- # print(so_yrs_coding$stdres) --> --&gt; <!-- <!-- #   # Women - 0-2 years, 3-5 years --> --&gt; <!-- <!-- #   # Men - 30+ years, 12-14 years, 15-17 years, 27-29 years, 9-11 years --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Years coding professionally --> --&gt; <!-- <!-- # so_tbl_years_coding_prof = table(so_mf_df$Gender, so_mf_df$YearsCodingProf)  --> --&gt; <!-- <!-- # so_yrs_coding_prof <- chisq.test(so_tbl_years_coding_prof, correct=TRUE) --> --&gt; <!-- <!-- # print(so_yrs_coding_prof) --> --&gt; <!-- <!-- # print(so_yrs_coding_prof$stdres) --> --&gt; <!-- <!-- #   # Women - 0-2 years, 3-5 years --> --&gt; <!-- <!-- #   # Men - 12-14 years, 9-11 years, 6-8 years, 15-17 years, 18-20 years, 30+ years --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Formal Education --> --&gt; <!-- <!-- # so_tbl_formal_ed = table(so_mf_df$Gender, so_mf_df$FormalEducation)  --> --&gt; <!-- <!-- # so_formal_ed <- chisq.test(so_tbl_formal_ed, correct=TRUE) --> --&gt; <!-- <!-- # print(so_formal_ed) --> --&gt; <!-- <!-- # print(so_formal_ed$stdres) --> --&gt; <!-- <!-- #   # Women - Bachelors, Masters --> --&gt; <!-- <!-- #   # Men - some college, Secondary, Primary/elementary --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # ### Github ### --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Formal Education --> --&gt; <!-- <!-- # gh_tbl_formal_ed = table(gh_mf_df$GENDER, gh_mf_df$FORMAL.EDUCATION)  --> --&gt; <!-- <!-- # gh_formal_ed <- chisq.test(gh_tbl_formal_ed, correct=TRUE)  --> --&gt; <!-- <!-- # print(gh_formal_ed) --> --&gt; <!-- <!-- # # N.S. --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Age of First Computer --> --&gt; <!-- <!-- # gh_tbl_age_computer = table(gh_mf_df$GENDER, gh_mf_df$AGE.AT.FIRST.COMPUTER.INTERNET)  --> --&gt; <!-- <!-- # gh_age_computer <- chisq.test(gh_tbl_age_computer, correct=TRUE)  --> --&gt; <!-- <!-- # print(gh_age_computer) --> --&gt; <!-- <!-- # # N.S. --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # ### HackerRank ### --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Formal Education --> --&gt; <!-- <!-- # hr_tbl_formal_ed = table(hr_mf_df$q3Gender, hr_mf_df$q4Education)  --> --&gt; <!-- <!-- # hr_formal_ed <- chisq.test(hr_tbl_formal_ed, correct=TRUE)  --> --&gt; <!-- <!-- # print(hr_formal_ed) --> --&gt; <!-- <!-- # print(hr_formal_ed$stdres) --> --&gt; <!-- <!-- #   # Women - Postgraduate, Some college --> --&gt; <!-- <!-- #   # Male - High school graduate, Vocational training --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Age Began Coding --> --&gt; <!-- <!-- # hr_tbl_age_coding = table(hr_mf_df$q3Gender, hr_mf_df$q1AgeBeginCoding)  --> --&gt; <!-- <!-- # hr_age_coding <- chisq.test(hr_tbl_age_coding, correct=TRUE)  --> --&gt; <!-- <!-- # print(hr_age_coding) --> --&gt; <!-- <!-- # print(hr_age_coding$stdres) --> --&gt; <!-- <!-- #   # Women - 16-20 yrs, 31-35, 41-50, 36-40, 26-30 --> --&gt; <!-- <!-- #   # Men - 11-15 yrs, 5-10 yrs --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Learn to code at University --> --&gt; <!-- <!-- # hr_tbl_learn_uni = table(hr_mf_df$q3Gender, hr_mf_df$q6LearnCodeUni)  --> --&gt; <!-- <!-- # hr_learn_uni <- chisq.test(hr_tbl_learn_uni, correct=TRUE)  --> --&gt; <!-- <!-- # print(hr_learn_uni) --> --&gt; <!-- <!-- # print(hr_learn_uni$stdres) --> --&gt; <!-- <!-- #   # Women - Yes --> --&gt; <!-- <!-- #   # Men - No --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- # # Learn to code - self-taught --> --&gt; <!-- <!-- # hr_tbl_learn_self = table(hr_mf_df$q3Gender, hr_mf_df$q6LearnCodeSelfTaught)  --> --&gt; <!-- <!-- # hr_learn_self <- chisq.test(hr_tbl_learn_self, correct=TRUE)  --> --&gt; <!-- <!-- # print(hr_learn_self) --> --&gt; <!-- <!-- # print(hr_learn_self$stdres) --> --&gt; <!-- <!-- #   # Men - Yes --> --&gt; <!-- <!-- #   # Women - No --> --&gt; <!-- <!-- #  --> --&gt; <!-- <!-- ``` --> --&gt;

Post-Hoc Correction for Multiple Comparisons
--------------------------------------------

``` r
# Stack Overflow p-values
so_pvalues <- c(2.2e-16, 2.2e-16, 2.2e-16, 0.0004998,  2.2e-16, 2.2e-16, 2.2e-16, 0.001176, 2.2e-16, 2.2e-16, 2.2e-16, 2.2e-16)
p.adjust(so_pvalues,method="BH")
```

    ##  [1] 2.640000e-16 2.640000e-16 2.640000e-16 5.452364e-04 2.640000e-16
    ##  [6] 2.640000e-16 2.640000e-16 1.176000e-03 2.640000e-16 2.640000e-16
    ## [11] 2.640000e-16 2.640000e-16

``` r
# GitHub p-values
gh_pvalues <- c(0.0002434, 6.429e-08, 0.002499, 0.1897, 0.02649, 0.6291, 0.004837, 2.628e-07, 0.1059, 2.642e-05, 3.251e-08, 0.001999, 0.1839, 0.6011)
p.adjust(gh_pvalues,method="BH")
```

    ##  [1] 6.815200e-04 4.500300e-07 4.998000e-03 2.213167e-01 4.120667e-02
    ##  [6] 6.291000e-01 8.464750e-03 1.226400e-06 1.482600e-01 9.247000e-05
    ## [11] 4.500300e-07 4.664333e-03 2.213167e-01 6.291000e-01

``` r
# HackerRank p-values
hr_pvalues <- c(2.2e-16, 2.2e-16, 2.2e-16, 2.2e-16, 2.986e-10, 2.2e-16, 2.2e-16, 2.2e-16)
p.adjust(hr_pvalues,method="BH")
```

    ## [1] 2.514286e-16 2.514286e-16 2.514286e-16 2.514286e-16 2.986000e-10
    ## [6] 2.514286e-16 2.514286e-16 2.514286e-16
