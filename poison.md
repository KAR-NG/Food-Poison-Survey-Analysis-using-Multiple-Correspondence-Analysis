poison_survey
================
Kar
2022-05-05

-   [SUMMARY](#summary)
-   [R PACKAGES](#r-packages)
-   [INTRODUCTION](#introduction)
-   [DATA PREPARATION](#data-preparation)
    -   [Data import](#data-import)
    -   [Data Description](#data-description)
    -   [Data exploration](#data-exploration)
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

### Data import

Following code download the dataset from the package.

``` r
data(poison)
```

Random sampling the first 10 rows of the dataset:

``` r
sample_n(poison, 10)
```

    ##    Age Time   Sick Sex   Nausea Vomiting Abdominals   Fever   Diarrhae   Potato
    ## 28  85    9 Sick_y   M Nausea_n  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 50   7   22 Sick_y   F Nausea_n  Vomit_n     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 20  11    0 Sick_n   F Nausea_n  Vomit_n     Abdo_n Fever_n Diarrhea_n Potato_y
    ## 35   7   15 Sick_y   F Nausea_y  Vomit_n     Abdo_y Fever_y Diarrhea_y Potato_n
    ## 15   8    0 Sick_n   M Nausea_n  Vomit_n     Abdo_n Fever_n Diarrhea_n Potato_y
    ## 3    6   16 Sick_y   F Nausea_n  Vomit_y     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 2    5    0 Sick_n   F Nausea_n  Vomit_n     Abdo_n Fever_n Diarrhea_n Potato_y
    ## 1    9   22 Sick_y   F Nausea_y  Vomit_n     Abdo_y Fever_y Diarrhea_y Potato_y
    ## 53   6    0 Sick_n   F Nausea_n  Vomit_n     Abdo_n Fever_n Diarrhea_n Potato_y
    ## 12  10   16 Sick_y   F Nausea_n  Vomit_y     Abdo_y Fever_n Diarrhea_n Potato_y
    ##      Fish   Mayo Courgette   Cheese   Icecream
    ## 28 Fish_y Mayo_n   Courg_n Cheese_y Icecream_y
    ## 50 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 20 Fish_y Mayo_n   Courg_y Cheese_y Icecream_y
    ## 35 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 15 Fish_y Mayo_n   Courg_y Cheese_n Icecream_y
    ## 3  Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 2  Fish_y Mayo_y   Courg_y Cheese_n Icecream_y
    ## 1  Fish_y Mayo_y   Courg_y Cheese_y Icecream_y
    ## 53 Fish_y Mayo_n   Courg_y Cheese_n Icecream_n
    ## 12 Fish_y Mayo_y   Courg_y Cheese_y Icecream_y

### Data Description

### Data exploration

## PRINCIPAL COMPONENT METHODS

## REFERENCE

KASSAMBARA A 2017, *Practical Guide To Principal Component Methods in
R*, Edition 1, sthda.com
