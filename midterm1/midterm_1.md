---
title: "Midterm 1"
author: "Evan Roybal"
date: "2022-01-27"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library(skimr)
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  

We have practiced using methods and processes to extract knowledge and insights from data. This has consisted of things such as working with data frames to test for NAs, figuring out how many unique values are in a column using unique(), using summarize() to gain small insights such as the mean or standard deviation of a dataframe column, or using group_by() in conjunction with summarize() to gain insights into sub-categories of a data frame. One example of this might be grouping the penguins data frame by species and then using summarize() and mean() on their beak length to gain insights into the average beak length of each species of penguin included in the data frame.

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  

The use of the summarize() function in conjunction with group_by() has so far been the most helpful thing I've learned as it is extraordinarily useful for quickly gaining insights into a data frame. In close second though is the 'na.rm = T' line that was taught as that makes it much easier to work with data. For me personally, I think that just simple manipulation and exploration of data frames needs more practice. While I know how to do it I feel that more practice is necessary to start to better understand how to use different functions in conjunction with each other and learn how to efficiently explore data.

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.


```r
elephants <- read.csv('data/ElephantsMF.csv')
```


```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1~
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,~
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"~
```


4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.


```r
elephants <- clean_names(elephants)
colnames(elephants)
```

```
## [1] "age"    "height" "sex"
```


```r
elephants$sex <- as.factor(elephants$sex)
is.factor(elephants$sex)
```

```
## [1] TRUE
```


5. (2 points) How many male and female elephants are represented in the data?


```r
unique(elephants$sex)
```

```
## [1] M F
## Levels: F M
```

```r
table(elephants$sex)
```

```
## 
##   F   M 
## 150 138
```

```r
150+138
```

```
## [1] 288
```
288 male and female elephants are represented in the data with 150 male elephants and 138 female elephants.

####

6. (2 points) What is the average age all elephants in the data?


```r
mean(elephants$age, na.rm = T)
```

```
## [1] 10.97132
```

The average age of all elephants in the data is 10.97 years.

7. (2 points) How does the average age and height of elephants compare by sex?


```r
mean_age_height <- elephants %>%
  group_by(sex) %>%
  summarize(across(c(age, height), mean, na.rm = T), total = n())
mean_age_height
```

```
## # A tibble: 2 x 4
##   sex     age height total
##   <fct> <dbl>  <dbl> <int>
## 1 F     12.8    190.   150
## 2 M      8.95   185.   138
```


8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  


```r
height_older <- elephants %>%
  filter(age > 20) %>%
  group_by(sex) %>%
  summarize(average_height = mean(height), 
            min_height = min(height), 
            max_height = max(height), 
            total = n())
height_older
```

```
## # A tibble: 2 x 5
##   sex   average_height min_height max_height total
##   <fct>          <dbl>      <dbl>      <dbl> <int>
## 1 F               232.       193.       278.    37
## 2 M               270.       229.       304.    13
```


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.


```r
vertebrate <- read.csv('data/IvindoData_DryadVersion.csv')
```


```r
colnames(vertebrate)
```

```
##  [1] "TransectID"              "Distance"               
##  [3] "HuntCat"                 "NumHouseholds"          
##  [5] "LandUse"                 "Veg_Rich"               
##  [7] "Veg_Stems"               "Veg_liana"              
##  [9] "Veg_DBH"                 "Veg_Canopy"             
## [11] "Veg_Understory"          "RA_Apes"                
## [13] "RA_Birds"                "RA_Elephant"            
## [15] "RA_Monkeys"              "RA_Rodent"              
## [17] "RA_Ungulate"             "Rich_AllSpecies"        
## [19] "Evenness_AllSpecies"     "Diversity_AllSpecies"   
## [21] "Rich_BirdSpecies"        "Evenness_BirdSpecies"   
## [23] "Diversity_BirdSpecies"   "Rich_MammalSpecies"     
## [25] "Evenness_MammalSpecies"  "Diversity_MammalSpecies"
```

```r
vertebrate$HuntCat <- as.factor(vertebrate$HuntCat)
vertebrate$LandUse <- as.factor(vertebrate$LandUse)
```


```r
glimpse(vertebrate)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <int> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16, ~
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.06~
## $ HuntCat                 <fct> Moderate, None, None, None, None, None, None, ~
## $ NumHouseholds           <int> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 56~
## $ LandUse                 <fct> Park, Park, Park, Logging, Park, Park, Park, L~
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.7~
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.2~
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, 8~
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.2~
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.00~
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.71~
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.1~
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.6~
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.43~
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.2~
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.26~
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, 8~
## $ Rich_AllSpecies         <int> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 22~
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.81~
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.56~
## $ Rich_BirdSpecies        <int> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, 1~
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.80~
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.92~
## $ Rich_MammalSpecies      <int> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11, ~
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.77~
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.92~
```


10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?


```r
high_moderate_hunting <- vertebrate %>%
  filter(HuntCat == 'Moderate' | HuntCat == 'High') %>%
  group_by(HuntCat) %>%
  summarize(average_diversity_birds = mean(Diversity_BirdSpecies, na.rm = T), 
            average_diversity_mammals = mean(Diversity_MammalSpecies, na.rm = T), total = n())
high_moderate_hunting
```

```
## # A tibble: 2 x 4
##   HuntCat  average_diversity_birds average_diversity_mammals total
##   <fct>                      <dbl>                     <dbl> <int>
## 1 High                        1.66                      1.74     7
## 2 Moderate                    1.62                      1.68     8
```


11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  


```r
short_distance <- vertebrate %>%
  filter(Distance < 3) %>%
  summarize(across(c(RA_Apes, RA_Birds, RA_Elephant, RA_Monkeys, RA_Rodent, RA_Ungulate), mean, na.rm = T), total = n())

long_distance <- vertebrate %>%
   filter(Distance > 25) %>%
  summarize(across(c(RA_Apes, RA_Birds, RA_Elephant, RA_Monkeys, RA_Rodent, RA_Ungulate), mean, na.rm = T), total = n())

short_distance
```

```
##   RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate total
## 1    0.12    76.64       0.145     17.335     3.895        1.87     2
```

```r
long_distance
```

```
##   RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate total
## 1    4.91    31.56           0      54.12      1.29        8.12     1
```

The relative abundance of apes, monkeys, and ungulates increases further away from the village but the relative abundance of birds, elephants, and rodents increases closer to the village.

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`


```r
land_use_impact <- vertebrate %>%
  filter(LandUse != 'Neither') %>%
  group_by(LandUse) %>%
  summarize(across(c(Veg_Rich, Rich_AllSpecies, Evenness_AllSpecies, Diversity_AllSpecies), mean, na.rm = T), 
            total = n())
land_use_impact
```

```
## # A tibble: 2 x 6
##   LandUse Veg_Rich Rich_AllSpecies Evenness_AllSpecies Diversity_AllSpeci~ total
##   <fct>      <dbl>           <dbl>               <dbl>               <dbl> <int>
## 1 Logging     14.4            19.6               0.753                2.23    13
## 2 Park        16.3            21.9               0.787                2.43     7
```

