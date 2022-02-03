---
title: "Lab 8 Homework"
author: "Evan Roybal"
date: "2022-02-03"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(naniar)
library(skimr)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- read.csv('data/sydneybeaches.csv') %>% janitor::clean_names()
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at C:/Users/royba/OneDrive - University of California, Davis/Backup/Documents/github/BIS15W2022_eroybal
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?


```r
skim(sydneybeaches)
```


Table: Data summary

|                         |              |
|:------------------------|:-------------|
|Name                     |sydneybeaches |
|Number of rows           |3690          |
|Number of columns        |8             |
|_______________________  |              |
|Column type frequency:   |              |
|character                |4             |
|numeric                  |4             |
|________________________ |              |
|Group variables          |None          |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|region        |         0|             1|  25|  25|     0|        1|          0|
|council       |         0|             1|  16|  16|     0|        2|          0|
|site          |         0|             1|  11|  23|     0|       11|          0|
|date          |         0|             1|  10|  10|     0|      344|          0|


**Variable type: numeric**

|skim_variable         | n_missing| complete_rate|   mean|     sd|     p0|    p25|    p50|    p75|    p100|hist                                     |
|:---------------------|---------:|-------------:|------:|------:|------:|------:|------:|------:|-------:|:----------------------------------------|
|beach_id              |         0|          1.00|  25.87|   2.08|  22.00|  24.00|  26.00|  27.40|   29.00|▆▃▇▇▆ |
|longitude             |         0|          1.00| 151.26|   0.01| 151.25| 151.26| 151.26| 151.27|  151.28|▅▇▂▆▂ |
|latitude              |         0|          1.00| -33.93|   0.03| -33.98| -33.95| -33.92| -33.90|  -33.89|▆▇▁▇▇ |
|enterococci_cfu_100ml |        29|          0.99|  33.92| 154.92|   0.00|   1.00|   5.00|  17.00| 4900.00|▇▁▁▁▁ |

```r
glimpse(sydneybeaches)
```

```
## Rows: 3,690
## Columns: 8
## $ beach_id              <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, ~
## $ region                <chr> "Sydney City Ocean Beaches", "Sydney City Ocean ~
## $ council               <chr> "Randwick Council", "Randwick Council", "Randwic~
## $ site                  <chr> "Clovelly Beach", "Clovelly Beach", "Clovelly Be~
## $ longitude             <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.2675~
## $ latitude              <dbl> -33.91449, -33.91449, -33.91449, -33.91449, -33.~
## $ date                  <chr> "02/01/2013", "06/01/2013", "12/01/2013", "18/01~
## $ enterococci_cfu_100ml <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8, 3,~
```

```r
head(sydneybeaches)
```

```
## # A tibble: 6 x 8
##   beach_id region    council   site    longitude latitude date  enterococci_cfu~
##      <dbl> <chr>     <chr>     <chr>       <dbl>    <dbl> <chr>            <dbl>
## 1       25 Sydney C~ Randwick~ Clovel~      151.    -33.9 02/0~               19
## 2       25 Sydney C~ Randwick~ Clovel~      151.    -33.9 06/0~                3
## 3       25 Sydney C~ Randwick~ Clovel~      151.    -33.9 12/0~                2
## 4       25 Sydney C~ Randwick~ Clovel~      151.    -33.9 18/0~               13
## 5       25 Sydney C~ Randwick~ Clovel~      151.    -33.9 30/0~                8
## 6       25 Sydney C~ Randwick~ Clovel~      151.    -33.9 05/0~                7
```

The data is "tidy", this is known by it being in long format with variables as columns and observations as rows.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long <- sydneybeaches %>%
  select(site, date, enterococci_cfu_100ml)
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches_long %>%
  pivot_wider(
  names_from = c(date), 
  values_from = enterococci_cfu_100ml)
head(sydneybeaches_wide)
```

```
## # A tibble: 6 x 345
##   site          `02/01/2013` `06/01/2013` `12/01/2013` `18/01/2013` `30/01/2013`
##   <chr>                <dbl>        <dbl>        <dbl>        <dbl>        <dbl>
## 1 Clovelly Bea~           19            3            2           13            8
## 2 Coogee Beach            15            4           17           18           22
## 3 Gordons Bay ~           NA           NA           NA           NA           NA
## 4 Little Bay B~            9            3           72            1           44
## 5 Malabar Beach            2            4          390           15           13
## 6 Maroubra Bea~            1            1           20            2           11
## # ... with 339 more variables: 05/02/2013 <dbl>, 11/02/2013 <dbl>,
## #   23/02/2013 <dbl>, 07/03/2013 <dbl>, 25/03/2013 <dbl>, 02/04/2013 <dbl>,
## #   12/04/2013 <dbl>, 18/04/2013 <dbl>, 24/04/2013 <dbl>, 01/05/2013 <dbl>,
## #   20/05/2013 <dbl>, 31/05/2013 <dbl>, 06/06/2013 <dbl>, 12/06/2013 <dbl>,
## #   24/06/2013 <dbl>, 06/07/2013 <dbl>, 18/07/2013 <dbl>, 24/07/2013 <dbl>,
## #   08/08/2013 <dbl>, 22/08/2013 <dbl>, 29/08/2013 <dbl>, 24/01/2013 <dbl>,
## #   17/02/2013 <dbl>, 01/03/2013 <dbl>, 13/03/2013 <dbl>, 19/03/2013 <dbl>, ...
```


5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide %>%
  pivot_longer(
    -site,
    names_to = 'date',
    values_to = 'enterococci_cfu_100ml')
```

```
## # A tibble: 3,784 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,774 more rows
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_datesep <- sydneybeaches_long %>%
  separate(date,
           into = c('day', 'month', 'year'),
           sep = '/')
head(sydneybeaches_datesep)
```

```
## # A tibble: 6 x 5
##   site           day   month year  enterococci_cfu_100ml
##   <chr>          <chr> <chr> <chr>                 <dbl>
## 1 Clovelly Beach 02    01    2013                     19
## 2 Clovelly Beach 06    01    2013                      3
## 3 Clovelly Beach 12    01    2013                      2
## 4 Clovelly Beach 18    01    2013                     13
## 5 Clovelly Beach 30    01    2013                      8
## 6 Clovelly Beach 05    02    2013                      7
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
sydneybeach_mean_cfu_year <- sydneybeaches_datesep %>%
  select(year, enterococci_cfu_100ml) %>%
  group_by(year) %>%
  summarise(average_cfu = mean(enterococci_cfu_100ml, na.rm = T), total = n())
sydneybeach_mean_cfu_year
```

```
## # A tibble: 6 x 3
##   year  average_cfu total
##   <chr>       <dbl> <int>
## 1 2013         50.6   602
## 2 2014         26.3   582
## 3 2015         31.2   627
## 4 2016         42.2   656
## 5 2017         20.7   673
## 6 2018         33.1   550
```


8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
easyread_mean_cfu_year <- sydneybeach_mean_cfu_year %>%
  select(-total) %>%
  pivot_wider(names_from = year,
             values_from = average_cfu)
easyread_mean_cfu_year
```

```
## # A tibble: 1 x 6
##   `2013` `2014` `2015` `2016` `2017` `2018`
##    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1   50.6   26.3   31.2   42.2   20.7   33.1
```


9. What was the most polluted beach in 2018?

```r
sydneybeaches_datesep %>%
  filter(year == 2018) %>%
  group_by(site) %>%
  summarise(average_cfu = mean(enterococci_cfu_100ml,na.rm = T)) %>%
  arrange(desc(average_cfu))
```

```
## # A tibble: 11 x 2
##    site                    average_cfu
##    <chr>                         <dbl>
##  1 South Maroubra Rockpool      112.  
##  2 Little Bay Beach              59.1 
##  3 Bronte Beach                  43.4 
##  4 Malabar Beach                 38.0 
##  5 Bondi Beach                   22.9 
##  6 Coogee Beach                  21.6 
##  7 Gordons Bay (East)            17.6 
##  8 Tamarama Beach                15.5 
##  9 South Maroubra Beach          12.5 
## 10 Clovelly Beach                10.6 
## 11 Maroubra Beach                 9.21
```

South Maroubra Rockpool was the most polluted beach in 2018.

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
