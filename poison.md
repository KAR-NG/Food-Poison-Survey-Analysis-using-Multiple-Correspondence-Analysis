poison_survey
================
Kar
2022-05-05

-   [SUMMARY](#summary)
-   [R PACKAGES](#r-packages)
-   [INTRODUCTION](#introduction)
-   [DATA PREPARATION](#data-preparation)
    -   [Data Import](#data-import)
    -   [Data Exploration](#data-exploration)
-   [EDA](#eda)
    -   [Missing](#missing)
-   [PRINCIPAL COMPONENT METHODS](#principal-component-methods)
-   [REFERENCE](#reference)

## SUMMARY

## R PACKAGES

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.4     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.1     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(skimr)
library(factoextra)
```

    ## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

``` r
library(FactoMineR)
```

## INTRODUCTION

This project will analyse a survey dataset using one of the principal
component methods. The technique of “Principal component methods” is
known as one of the machine learning methods, belonging to the
“unsupervised” branch, which can quickly summarise a dataset by
revealing the most important variables (columns) or observations (rows)
in the dataset.

The data of this survey dataset was collected from students in a primary
school who suffered from food poisoning. The dataset contains various
type of information such as age, time, whether the student is sicked,
sex, symptoms of food poisoning (nausea, vomiting, abdominal, fever, and
diarrhea) and food they ate (potato, fish, mayo, courgette, cheese, ice
cream).

## DATA PREPARATION

The dataset of this project is called “poison”, which is a public
dataset from the R package “FactoMineR”.

### Data Import

Following code download the dataset from the package.

``` r
data(poison)
```

Randomly sample 10 rows within the dataset:

``` r
sample_n(poison, 10)
```

    ##    Age Time   Sick Sex   Nausea Vomiting Abdominals   Fever   Diarrhae   Potato
    ## 12  10   16 Sick_y   F Nausea_n  Vomit_y     Abdo_y Fever_n Diarrhea_n Potato_y
    ## 23   8   21 Sick_y   F Nausea_n  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 32   6    0 Sick_n   M Nausea_n  Vomit_n     Abdo_n Fever_n Diarrhea_n Potato_y
    ## 30  79   17 Sick_y   F Nausea_y  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 45   5   20 Sick_y   M Nausea_n  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 9    5   20 Sick_y   M Nausea_y  Vomit_n     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 11   7   17 Sick_y   F Nausea_y  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 18   5   15 Sick_y   M Nausea_y  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 8   10    8 Sick_y   F Nausea_y  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 43  82   20 Sick_y   F Nausea_n  Vomit_n     Abdo_y Fever_y Diarrhea_y Potato_y
    ##      Fish   Mayo Courgette   Cheese   Icecream
    ## 12 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 23 Fish_y Mayo_y   Courg_n Cheese_y Icecream_y
    ## 32 Fish_y Mayo_n   Courg_y Cheese_n Icecream_y
    ## 30 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 45 Fish_y Mayo_y   Courg_n Cheese_y Icecream_y
    ## 9  Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 11 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 18 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 8  Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 43 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y

### Data Exploration

The dataset has 55 rows (55 students) and 15 columns recording the
various type of variables (also known as feature).

Following code also shows the variable type allocated by R to each of
the column.

``` r
glimpse(poison)
```

    ## Rows: 55
    ## Columns: 15
    ## $ Age        <int> 9, 5, 6, 9, 7, 72, 5, 10, 5, 11, 7, 10, 36, 9, 8, 6, 7, 5, ~
    ## $ Time       <int> 22, 0, 16, 0, 14, 9, 16, 8, 20, 12, 17, 16, 19, 0, 0, 6, 10~
    ## $ Sick       <fct> Sick_y, Sick_n, Sick_y, Sick_n, Sick_y, Sick_y, Sick_y, Sic~
    ## $ Sex        <fct> F, F, F, F, M, M, F, F, M, M, F, F, F, F, M, F, M, M, F, F,~
    ## $ Nausea     <fct> Nausea_y, Nausea_n, Nausea_n, Nausea_n, Nausea_n, Nausea_n,~
    ## $ Vomiting   <fct> Vomit_n, Vomit_n, Vomit_y, Vomit_n, Vomit_y, Vomit_n, Vomit~
    ## $ Abdominals <fct> Abdo_y, Abdo_n, Abdo_y, Abdo_n, Abdo_y, Abdo_y, Abdo_y, Abd~
    ## $ Fever      <fct> Fever_y, Fever_n, Fever_y, Fever_n, Fever_y, Fever_y, Fever~
    ## $ Diarrhae   <fct> Diarrhea_y, Diarrhea_n, Diarrhea_y, Diarrhea_n, Diarrhea_y,~
    ## $ Potato     <fct> Potato_y, Potato_y, Potato_y, Potato_y, Potato_y, Potato_y,~
    ## $ Fish       <fct> Fish_y, Fish_y, Fish_y, Fish_y, Fish_y, Fish_n, Fish_y, Fis~
    ## $ Mayo       <fct> Mayo_y, Mayo_y, Mayo_y, Mayo_n, Mayo_y, Mayo_y, Mayo_y, May~
    ## $ Courgette  <fct> Courg_y, Courg_y, Courg_y, Courg_y, Courg_y, Courg_y, Courg~
    ## $ Cheese     <fct> Cheese_y, Cheese_n, Cheese_y, Cheese_y, Cheese_y, Cheese_y,~
    ## $ Icecream   <fct> Icecream_y, Icecream_y, Icecream_y, Icecream_y, Icecream_y,~

Factor “fct” is a variable type we usually allocate to variables that
have categorisation nature. There is usually a conversion step required
to perform by analyst to convert character “chr” variable such as “Yes”
and “No” into factor “fct”. This step seems like has been completed.

Insights from following summary,

-   Respondents have age between 4 to 88, with majority of them fall
    between 6 to 10 years old with a median of 8. Mean of 16.93
    shouldn’t be used for reference because 88 is a outlier.  
-   Time should be time of recording the data.  
-   The rest of the variables have two categories of either “Yes” or
    “No”.

``` r
summary(poison)
```

    ##       Age             Time           Sick    Sex         Nausea      Vomiting 
    ##  Min.   : 4.00   Min.   : 0.00   Sick_n:17   F:28   Nausea_n:43   Vomit_n:33  
    ##  1st Qu.: 6.00   1st Qu.: 0.00   Sick_y:38   M:27   Nausea_y:12   Vomit_y:22  
    ##  Median : 8.00   Median :12.00                                                
    ##  Mean   :16.93   Mean   :10.16                                                
    ##  3rd Qu.:10.00   3rd Qu.:16.50                                                
    ##  Max.   :88.00   Max.   :22.00                                                
    ##   Abdominals     Fever          Diarrhae       Potato       Fish        Mayo   
    ##  Abdo_n:18   Fever_n:20   Diarrhea_n:20   Potato_n: 3   Fish_n: 1   Mayo_n:10  
    ##  Abdo_y:37   Fever_y:35   Diarrhea_y:35   Potato_y:52   Fish_y:54   Mayo_y:45  
    ##                                                                                
    ##                                                                                
    ##                                                                                
    ##                                                                                
    ##    Courgette       Cheese         Icecream 
    ##  Courg_n: 5   Cheese_n: 7   Icecream_n: 4  
    ##  Courg_y:50   Cheese_y:48   Icecream_y:51  
    ##                                            
    ##                                            
    ##                                            
    ## 

Following is an alternative to see the structure of the dataset. Other
than Age and time, the rest of the variables are factor type with 2
levels, of either “Y” or “No”, which stand for “Yes” or “No”.

``` r
str(poison)
```

    ## 'data.frame':    55 obs. of  15 variables:
    ##  $ Age       : int  9 5 6 9 7 72 5 10 5 11 ...
    ##  $ Time      : int  22 0 16 0 14 9 16 8 20 12 ...
    ##  $ Sick      : Factor w/ 2 levels "Sick_n","Sick_y": 2 1 2 1 2 2 2 2 2 2 ...
    ##  $ Sex       : Factor w/ 2 levels "F","M": 1 1 1 1 2 2 1 1 2 2 ...
    ##  $ Nausea    : Factor w/ 2 levels "Nausea_n","Nausea_y": 2 1 1 1 1 1 1 2 2 1 ...
    ##  $ Vomiting  : Factor w/ 2 levels "Vomit_n","Vomit_y": 1 1 2 1 2 1 2 2 1 2 ...
    ##  $ Abdominals: Factor w/ 2 levels "Abdo_n","Abdo_y": 2 1 2 1 2 2 2 2 2 1 ...
    ##  $ Fever     : Factor w/ 2 levels "Fever_n","Fever_y": 2 1 2 1 2 2 2 2 2 2 ...
    ##  $ Diarrhae  : Factor w/ 2 levels "Diarrhea_n","Diarrhea_y": 2 1 2 1 2 2 2 2 2 2 ...
    ##  $ Potato    : Factor w/ 2 levels "Potato_n","Potato_y": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ Fish      : Factor w/ 2 levels "Fish_n","Fish_y": 2 2 2 2 2 1 2 2 2 2 ...
    ##  $ Mayo      : Factor w/ 2 levels "Mayo_n","Mayo_y": 2 2 2 1 2 2 2 2 2 2 ...
    ##  $ Courgette : Factor w/ 2 levels "Courg_n","Courg_y": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ Cheese    : Factor w/ 2 levels "Cheese_n","Cheese_y": 2 1 2 2 2 2 2 2 2 2 ...
    ##  $ Icecream  : Factor w/ 2 levels "Icecream_n","Icecream_y": 2 2 2 2 2 2 2 2 2 2 ...

Checking missing data in the dataset.

``` r
skim_without_charts(poison)
```

|                                                  |        |
|:-------------------------------------------------|:-------|
| Name                                             | poison |
| Number of rows                                   | 55     |
| Number of columns                                | 15     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |        |
| Column type frequency:                           |        |
| factor                                           | 13     |
| numeric                                          | 2      |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |        |
| Group variables                                  | None   |

Data summary

**Variable type: factor**

| skim_variable | n_missing | complete_rate | ordered | n_unique | top_counts       |
|:--------------|----------:|--------------:|:--------|---------:|:-----------------|
| Sick          |         0 |             1 | FALSE   |        2 | Sic: 38, Sic: 17 |
| Sex           |         0 |             1 | FALSE   |        2 | F: 28, M: 27     |
| Nausea        |         0 |             1 | FALSE   |        2 | Nau: 43, Nau: 12 |
| Vomiting      |         0 |             1 | FALSE   |        2 | Vom: 33, Vom: 22 |
| Abdominals    |         0 |             1 | FALSE   |        2 | Abd: 37, Abd: 18 |
| Fever         |         0 |             1 | FALSE   |        2 | Fev: 35, Fev: 20 |
| Diarrhae      |         0 |             1 | FALSE   |        2 | Dia: 35, Dia: 20 |
| Potato        |         0 |             1 | FALSE   |        2 | Pot: 52, Pot: 3  |
| Fish          |         0 |             1 | FALSE   |        2 | Fis: 54, Fis: 1  |
| Mayo          |         0 |             1 | FALSE   |        2 | May: 45, May: 10 |
| Courgette     |         0 |             1 | FALSE   |        2 | Cou: 50, Cou: 5  |
| Cheese        |         0 |             1 | FALSE   |        2 | Che: 48, Che: 7  |
| Icecream      |         0 |             1 | FALSE   |        2 | Ice: 51, Ice: 4  |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |  mean |    sd |  p0 | p25 | p50 |  p75 | p100 |
|:--------------|----------:|--------------:|------:|------:|----:|----:|----:|-----:|-----:|
| Age           |         0 |             1 | 16.93 | 23.78 |   4 |   6 |   8 | 10.0 |   88 |
| Time          |         0 |             1 | 10.16 |  7.80 |   0 |   0 |  12 | 16.5 |   22 |

## EDA

### Missing

## PRINCIPAL COMPONENT METHODS

## REFERENCE

KASSAMBARA A 2017, *Practical Guide To Principal Component Methods in
R*, Edition 1, sthda.com
