---
title: "Lab 6 Homework"
author: "Evan Roybal"
date: "2022-01-23"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

variables:

```r
skim(fisheries)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |fisheries |
|Number of rows           |17692     |
|Number of columns        |71        |
|_______________________  |          |
|Column type frequency:   |          |
|character                |69        |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable           | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-----------------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Country                 |         0|          1.00|   4|  25|     0|      204|          0|
|Common name             |         0|          1.00|   3|  30|     0|     1553|          0|
|ISSCAAP taxonomic group |         0|          1.00|   5|  36|     0|       30|          0|
|ASFIS species#          |         0|          1.00|  10|  13|     0|     1553|          0|
|ASFIS species name      |         0|          1.00|   6|  32|     0|     1548|          0|
|Measure                 |         0|          1.00|  17|  17|     0|        1|          0|
|1950                    |     15561|          0.12|   1|   6|     0|      513|          0|
|1951                    |     15550|          0.12|   1|   7|     0|      536|          0|
|1952                    |     15501|          0.12|   1|   7|     0|      553|          0|
|1953                    |     15439|          0.13|   1|   6|     0|      570|          0|
|1954                    |     15417|          0.13|   1|   7|     0|      588|          0|
|1955                    |     15382|          0.13|   1|   7|     0|      589|          0|
|1956                    |     15331|          0.13|   1|   7|     0|      633|          0|
|1957                    |     15253|          0.14|   1|   7|     0|      627|          0|
|1958                    |     15138|          0.14|   1|   6|     0|      643|          0|
|1959                    |     15110|          0.15|   1|   7|     0|      641|          0|
|1960                    |     15016|          0.15|   1|   7|     0|      673|          0|
|1961                    |     14922|          0.16|   1|   8|     0|      713|          0|
|1962                    |     14801|          0.16|   1|   8|     0|      729|          0|
|1963                    |     14707|          0.17|   1|   8|     0|      760|          0|
|1964                    |     14349|          0.19|   1|   8|     0|      759|          0|
|1965                    |     14241|          0.20|   1|   8|     0|      798|          0|
|1966                    |     14187|          0.20|   1|   8|     0|      801|          0|
|1967                    |     14047|          0.21|   1|   8|     0|      815|          0|
|1968                    |     13963|          0.21|   1|   8|     0|      829|          0|
|1969                    |     13920|          0.21|   1|   8|     0|      853|          0|
|1970                    |     13113|          0.26|   1|   8|     0|     1225|          0|
|1971                    |     12925|          0.27|   1|   8|     0|     1326|          0|
|1972                    |     12749|          0.28|   1|   8|     0|     1372|          0|
|1973                    |     12673|          0.28|   1|   8|     0|     1432|          0|
|1974                    |     12583|          0.29|   1|   8|     0|     2601|          0|
|1975                    |     12333|          0.30|   1|   8|     0|     2767|          0|
|1976                    |     12177|          0.31|   1|   8|     0|     2804|          0|
|1977                    |     12014|          0.32|   1|   8|     0|     2867|          0|
|1978                    |     11847|          0.33|   1|   8|     0|     2901|          0|
|1979                    |     11820|          0.33|   1|   8|     0|     2932|          0|
|1980                    |     11747|          0.34|   1|   8|     0|     2956|          0|
|1981                    |     11713|          0.34|   1|   8|     0|     2996|          0|
|1982                    |     11558|          0.35|   1|   9|     0|     3030|          0|
|1983                    |     11453|          0.35|   1|   8|     0|     3031|          0|
|1984                    |     11309|          0.36|   1|   8|     0|     3076|          0|
|1985                    |     11212|          0.37|   1|   8|     0|     3161|          0|
|1986                    |     11086|          0.37|   1|   8|     0|     3242|          0|
|1987                    |     10930|          0.38|   1|   8|     0|     3255|          0|
|1988                    |     10493|          0.41|   1|   8|     0|     3435|          0|
|1989                    |     10435|          0.41|   1|   8|     0|     3425|          0|
|1990                    |     10293|          0.42|   1|   8|     0|     3446|          0|
|1991                    |     10364|          0.41|   1|   8|     0|     3420|          0|
|1992                    |     10435|          0.41|   1|   8|     0|     3322|          0|
|1993                    |     10522|          0.41|   1|   8|     0|     3313|          0|
|1994                    |     10400|          0.41|   1|   8|     0|     3313|          0|
|1995                    |     10148|          0.43|   1|   8|     0|     3329|          0|
|1996                    |      9990|          0.44|   1|   7|     0|     3358|          0|
|1997                    |      9773|          0.45|   1|   9|     0|     3393|          0|
|1998                    |      9579|          0.46|   1|   9|     0|     3399|          0|
|1999                    |      9265|          0.48|   1|   9|     0|     3428|          0|
|2000                    |      8899|          0.50|   1|   9|     0|     3481|          0|
|2001                    |      8646|          0.51|   1|   9|     0|     3490|          0|
|2002                    |      8590|          0.51|   1|   9|     0|     3507|          0|
|2003                    |      8383|          0.53|   1|   9|     0|     3482|          0|
|2004                    |      7977|          0.55|   1|   9|     0|     3505|          0|
|2005                    |      7822|          0.56|   1|   9|     0|     3532|          0|
|2006                    |      7699|          0.56|   1|   9|     0|     3565|          0|
|2007                    |      7589|          0.57|   1|   8|     0|     3551|          0|
|2008                    |      7667|          0.57|   1|   8|     0|     3537|          0|
|2009                    |      7573|          0.57|   1|   8|     0|     3572|          0|
|2010                    |      7499|          0.58|   1|   8|     0|     3590|          0|
|2011                    |      7371|          0.58|   1|   8|     0|     3618|          0|
|2012                    |      7336|          0.59|   1|   8|     0|     3609|          0|


**Variable type: numeric**

|skim_variable          | n_missing| complete_rate|  mean|    sd| p0| p25| p50| p75| p100|hist                                     |
|:----------------------|---------:|-------------:|-----:|-----:|--:|---:|---:|---:|----:|:----------------------------------------|
|ISSCAAP group#         |         0|             1| 37.38|  7.88| 11|  33|  36|  38|   77|??????????????? |
|FAO major fishing area |         0|             1| 45.34| 18.33| 18|  31|  37|  57|   88|??????????????? |

```r
summary(fisheries)
```

```
##    Country          Common name        ISSCAAP group#  ISSCAAP taxonomic group
##  Length:17692       Length:17692       Min.   :11.00   Length:17692           
##  Class :character   Class :character   1st Qu.:33.00   Class :character       
##  Mode  :character   Mode  :character   Median :36.00   Mode  :character       
##                                        Mean   :37.38                          
##                                        3rd Qu.:38.00                          
##                                        Max.   :77.00                          
##  ASFIS species#     ASFIS species name FAO major fishing area
##  Length:17692       Length:17692       Min.   :18.00         
##  Class :character   Class :character   1st Qu.:31.00         
##  Mode  :character   Mode  :character   Median :37.00         
##                                        Mean   :45.34         
##                                        3rd Qu.:57.00         
##                                        Max.   :88.00         
##    Measure              1950               1951               1952          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1953               1954               1955               1956          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1957               1958               1959               1960          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1961               1962               1963               1964          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1965               1966               1967               1968          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1969               1970               1971               1972          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1973               1974               1975               1976          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1977               1978               1979               1980          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1981               1982               1983               1984          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1985               1986               1987               1988          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1989               1990               1991               1992          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1993               1994               1995               1996          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1997               1998               1999               2000          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2001               2002               2003               2004          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2005               2006               2007               2008          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2009               2010               2011               2012          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
## 
```

```r
glimpse(fisheries)
```

```
## Rows: 17,692
## Columns: 71
## $ Country                   <chr> "Albania", "Albania", "Albania", "Albania", ~
## $ `Common name`             <chr> "Angelsharks, sand devils nei", "Atlantic bo~
## $ `ISSCAAP group#`          <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, ~
## $ `ISSCAAP taxonomic group` <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, ~
## $ `ASFIS species#`          <chr> "10903XXXXX", "1750100101", "17710001XX", "2~
## $ `ASFIS species name`      <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp~
## $ `FAO major fishing area`  <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, ~
## $ Measure                   <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Q~
## $ `1950`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1951`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1952`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1953`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1954`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1955`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1956`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1957`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1958`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1959`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1960`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1961`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1962`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1963`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1964`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1965`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1966`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1967`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1968`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1969`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1970`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1971`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1972`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1973`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1974`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1975`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1976`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1977`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1978`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1979`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1980`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1981`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1982`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1983`                    <chr> NA, NA, NA, NA, NA, NA, "559", NA, NA, NA, N~
## $ `1984`                    <chr> NA, NA, NA, NA, NA, NA, "392", NA, NA, NA, N~
## $ `1985`                    <chr> NA, NA, NA, NA, NA, NA, "406", NA, NA, NA, N~
## $ `1986`                    <chr> NA, NA, NA, NA, NA, NA, "499", NA, NA, NA, N~
## $ `1987`                    <chr> NA, NA, NA, NA, NA, NA, "564", NA, NA, NA, N~
## $ `1988`                    <chr> NA, NA, NA, NA, NA, NA, "724", NA, NA, NA, N~
## $ `1989`                    <chr> NA, NA, NA, NA, NA, NA, "583", NA, NA, NA, N~
## $ `1990`                    <chr> NA, NA, NA, NA, NA, NA, "754", NA, NA, NA, N~
## $ `1991`                    <chr> NA, NA, NA, NA, NA, NA, "283", NA, NA, NA, N~
## $ `1992`                    <chr> NA, NA, NA, NA, NA, NA, "196", NA, NA, NA, N~
## $ `1993`                    <chr> NA, NA, NA, NA, NA, NA, "150 F", NA, NA, NA,~
## $ `1994`                    <chr> NA, NA, NA, NA, NA, NA, "100 F", NA, NA, NA,~
## $ `1995`                    <chr> "0 0", "1", NA, "0 0", "0 0", NA, "52", "30"~
## $ `1996`                    <chr> "53", "2", NA, "3", "2", NA, "104", "8", NA,~
## $ `1997`                    <chr> "20", "0 0", NA, "0 0", "0 0", NA, "65", "4"~
## $ `1998`                    <chr> "31", "12", NA, NA, NA, NA, "220", "18", NA,~
## $ `1999`                    <chr> "30", "30", NA, NA, NA, NA, "220", "18", NA,~
## $ `2000`                    <chr> "30", "25", "2", NA, NA, NA, "220", "20", NA~
## $ `2001`                    <chr> "16", "30", NA, NA, NA, NA, "120", "23", NA,~
## $ `2002`                    <chr> "79", "24", NA, "34", "6", NA, "150", "84", ~
## $ `2003`                    <chr> "1", "4", NA, "22", NA, NA, "84", "178", NA,~
## $ `2004`                    <chr> "4", "2", "2", "15", "1", "2", "76", "285", ~
## $ `2005`                    <chr> "68", "23", "4", "12", "5", "6", "68", "150"~
## $ `2006`                    <chr> "55", "30", "7", "18", "8", "9", "86", "102"~
## $ `2007`                    <chr> "12", "19", NA, NA, NA, NA, "132", "18", NA,~
## $ `2008`                    <chr> "23", "27", NA, NA, NA, NA, "132", "23", NA,~
## $ `2009`                    <chr> "14", "21", NA, NA, NA, NA, "154", "20", NA,~
## $ `2010`                    <chr> "78", "23", "7", NA, NA, NA, "80", "228", NA~
## $ `2011`                    <chr> "12", "12", NA, NA, NA, NA, "88", "9", NA, "~
## $ `2012`                    <chr> "5", "5", NA, NA, NA, NA, "129", "290", NA, ~
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- clean_names(fisheries)
```


```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.

204 countries are represented in the data.

```r
unique(fisheries$country)
```

```
##   [1] Albania                   Algeria                  
##   [3] American Samoa            Angola                   
##   [5] Anguilla                  Antigua and Barbuda      
##   [7] Argentina                 Aruba                    
##   [9] Australia                 Bahamas                  
##  [11] Bahrain                   Bangladesh               
##  [13] Barbados                  Belgium                  
##  [15] Belize                    Benin                    
##  [17] Bermuda                   Bonaire/S.Eustatius/Saba 
##  [19] Bosnia and Herzegovina    Brazil                   
##  [21] British Indian Ocean Ter  British Virgin Islands   
##  [23] Brunei Darussalam         Bulgaria                 
##  [25] Cabo Verde                Cambodia                 
##  [27] Cameroon                  Canada                   
##  [29] Cayman Islands            Channel Islands          
##  [31] Chile                     China                    
##  [33] China, Hong Kong SAR      China, Macao SAR         
##  [35] Colombia                  Comoros                  
##  [37] Congo, Dem. Rep. of the   Congo, Republic of       
##  [39] Cook Islands              Costa Rica               
##  [41] Croatia                   Cuba                     
##  [43] Cura<e7>ao                Cyprus                   
##  [45] C<f4>te d'Ivoire          Denmark                  
##  [47] Djibouti                  Dominica                 
##  [49] Dominican Republic        Ecuador                  
##  [51] Egypt                     El Salvador              
##  [53] Equatorial Guinea         Eritrea                  
##  [55] Estonia                   Ethiopia                 
##  [57] Falkland Is.(Malvinas)    Faroe Islands            
##  [59] Fiji, Republic of         Finland                  
##  [61] France                    French Guiana            
##  [63] French Polynesia          French Southern Terr     
##  [65] Gabon                     Gambia                   
##  [67] Georgia                   Germany                  
##  [69] Ghana                     Gibraltar                
##  [71] Greece                    Greenland                
##  [73] Grenada                   Guadeloupe               
##  [75] Guam                      Guatemala                
##  [77] Guinea                    GuineaBissau             
##  [79] Guyana                    Haiti                    
##  [81] Honduras                  Iceland                  
##  [83] India                     Indonesia                
##  [85] Iran (Islamic Rep. of)    Iraq                     
##  [87] Ireland                   Isle of Man              
##  [89] Israel                    Italy                    
##  [91] Jamaica                   Japan                    
##  [93] Jordan                    Kenya                    
##  [95] Kiribati                  Korea, Dem. People's Rep 
##  [97] Korea, Republic of        Kuwait                   
##  [99] Latvia                    Lebanon                  
## [101] Liberia                   Libya                    
## [103] Lithuania                 Madagascar               
## [105] Malaysia                  Maldives                 
## [107] Malta                     Marshall Islands         
## [109] Martinique                Mauritania               
## [111] Mauritius                 Mayotte                  
## [113] Mexico                    Micronesia, Fed.States of
## [115] Monaco                    Montenegro               
## [117] Montserrat                Morocco                  
## [119] Mozambique                Myanmar                  
## [121] Namibia                   Nauru                    
## [123] Netherlands               Netherlands Antilles     
## [125] New Caledonia             New Zealand              
## [127] Nicaragua                 Nigeria                  
## [129] Niue                      Norfolk Island           
## [131] Northern Mariana Is.      Norway                   
## [133] Oman                      Other nei                
## [135] Pakistan                  Palau                    
## [137] Palestine, Occupied Tr.   Panama                   
## [139] Papua New Guinea          Peru                     
## [141] Philippines               Pitcairn Islands         
## [143] Poland                    Portugal                 
## [145] Puerto Rico               Qatar                    
## [147] Romania                   Russian Federation       
## [149] R<e9>union                Saint Barth<e9>lemy      
## [151] Saint Helena              Saint Kitts and Nevis    
## [153] Saint Lucia               Saint Vincent/Grenadines 
## [155] SaintMartin               Samoa                    
## [157] Sao Tome and Principe     Saudi Arabia             
## [159] Senegal                   Serbia and Montenegro    
## [161] Seychelles                Sierra Leone             
## [163] Singapore                 Sint Maarten             
## [165] Slovenia                  Solomon Islands          
## [167] Somalia                   South Africa             
## [169] Spain                     Sri Lanka                
## [171] St. Pierre and Miquelon   Sudan                    
## [173] Sudan (former)            Suriname                 
## [175] Svalbard and Jan Mayen    Sweden                   
## [177] Syrian Arab Republic      Taiwan Province of China 
## [179] Tanzania, United Rep. of  Thailand                 
## [181] TimorLeste                Togo                     
## [183] Tokelau                   Tonga                    
## [185] Trinidad and Tobago       Tunisia                  
## [187] Turkey                    Turks and Caicos Is.     
## [189] Tuvalu                    US Virgin Islands        
## [191] Ukraine                   Un. Sov. Soc. Rep.       
## [193] United Arab Emirates      United Kingdom           
## [195] United States of America  Uruguay                  
## [197] Vanuatu                   Venezuela, Boliv Rep of  
## [199] Viet Nam                  Wallis and Futuna Is.    
## [201] Western Sahara            Yemen                    
## [203] Yugoslavia SFR            Zanzibar                 
## 204 Levels: Albania Algeria American Samoa Angola ... Zanzibar
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_refocus <- fisheries_tidy %>%
  select(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)
head(fisheries_refocus)
```

```
## # A tibble: 6 x 6
##   country isscaap_taxonomic_group asfis_species_na~ asfis_species_n~  year catch
##   <fct>   <chr>                   <chr>             <fct>            <dbl> <dbl>
## 1 Albania Sharks, rays, chimaeras Squatinidae       10903XXXXX        1995    NA
## 2 Albania Sharks, rays, chimaeras Squatinidae       10903XXXXX        1996    53
## 3 Albania Sharks, rays, chimaeras Squatinidae       10903XXXXX        1997    20
## 4 Albania Sharks, rays, chimaeras Squatinidae       10903XXXXX        1998    31
## 5 Albania Sharks, rays, chimaeras Squatinidae       10903XXXXX        1999    30
## 6 Albania Sharks, rays, chimaeras Squatinidae       10903XXXXX        2000    30
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

1553 distinct fish species.

```r
unique(fisheries_tidy$asfis_species_number)
```

```
##    [1] 10903XXXXX    1750100101    17710001XX    2280203101    1480403301   
##    [6] 1702021301    1703926101    2280100117    10801003XX    3210200202   
##   [11] 1703906006    3210900507    1830300701    2290100804    32104001XX   
##   [16] 17037XXXXX    2280101701    10901XXXXX    1210600201    1431300101   
##   [21] 1830204802    1480500401    2294200718    1210506401    1700634503   
##   [26] 1210506601    183XXXXXXX    1470100101    1703923508    17321XXXXX023
##   [31] 1702304801    1721201002    1901000201    17002042XX    11001XXXXX   
##   [36] 17802XXXXX    17023004XX    1620100101    1702307202    199XXXXXXX010
##   [41] 3161000112    2311109002    1830500301    19501001XX    16501XXXXX   
##   [46] 2294200602    17039008XX    17040075XX    1480403202    1060800301   
##   [51] 17039XXXXX    110XXXXXXX    14804028XX    1210501210    1703929301   
##   [56] 17501002XX    17801XXXXX    1703707001    1750600601    16302XXXXX   
##   [61] 10804007XX    6930400701    3161100105    17041007XX    1750400301   
##   [66] 1830509201    1700505801    1750102601    1750100205    1703900803   
##   [71] 321XXXXXXX    121XXXXXXX020 1703900802    2281201810    2282300303   
##   [76] 17501023XX018 1480400601    17002XXXXX    1750102401    17501XXXXX   
##   [81] 299XXXXXXX013 399XXXXXXX016 32109XXXXX    22901008XX    17039191XX   
##   [86] 1090100704    1750100601    1703919103    1703907601    1703927702   
##   [91] 12105012XX    199XXXXXXX054 1750102501    1703903303    1480403401   
##   [96] 1750102605    1750102612    1750300507    1750300505    1080200401   
##  [101] 17023XXXXX    1702807101    17038XXXXX    1750300402    10608002XX   
##  [106] 231XXXXXXX    1520100102    17065XXXXX    17027XXXXX    1750300905   
##  [111] 17032XXXXX    16111XXXXX    1750300903    17402XXXXX    10606006XX   
##  [116] 22901001XX    175XXXXXXX    1750101001    1750102610    17023048XX   
##  [121] 12106XXXXX    1702326801    1703626303    1703906302    1760300401   
##  [126] 1703707009    1702300413    1750100201    14313XXXXX    1702304429   
##  [131] 1702300414    32102XXXXX026 199XXXXXXX012 17039060XX    1702304442   
##  [136] 17036XXXXX    1703906002    1750600302    229XXXXXXX    1703710601   
##  [141] 228XXXXXXX    199XXXXXXX013 1760800310    1702304706    1703900807   
##  [146] 17039033XX    14102XXXXX    1480500419    18303XXXXX    1210501305   
##  [151] 1703710606    2280203102    17077XXXXX    32105XXXXX036 1750101511   
##  [156] 17037457XX    1704149903    1211200112    1830305701    2290100108   
##  [161] 30706002XX    17066XXXXX    2311000201    19001XXXXX    19009XXXXX   
##  [166] 17047XXXXX    2291504102    19010XXXXX    1709345201    1630200204   
##  [171] 1760301102    3162301004    3074200701    1831000201    17092XXXXX   
##  [176] 1100400132    1709201501    1210600206    1090300404    1431300102   
##  [181] 1703707003    1704100703    1480500406    2282900601    1700225901   
##  [186] 3210501003    2280106701    1720900602    17802020XX    1750300904   
##  [191] 18308046XX    1100403602    1703721002    1780101703    1709441601   
##  [196] 1480203001    1050200201    1480400801    1720820201    1210502401   
##  [201] 1720928801    1050200502    1100400233    31611020XX    1707027002   
##  [206] 1760801001    3161003801    1080201020    17094XXXXX    11007XXXXX   
##  [211] 1100400201    1100400154    1210506602    148XXXXXXX    14806001XX   
##  [216] 1709201906    1709201903    1703709601    13208XXXXX    1709441701   
##  [221] 1709201902    1100400202    14801001XX    1709446603    1080400712   
##  [226] 1900800201    1709243001    1702328301    1709200702    1480501701   
##  [231] 1780100148    1709244001    3160806702    1100400234    3210400102   
##  [236] 1709201502    1580200101    1120300102    11004XXXXX    1480600105   
##  [241] 1100403201    3161000108    1702300411    1060200501    3210506001   
##  [246] 1480100101    1100403601    1750500101    2302012301    1703903301   
##  [251] 1709441801    1480403302    1480500403    2302007001    1703703901   
##  [256] 1760300405    1100400146    1709252901    1703701617    1480204001   
##  [261] 1080401103    199XXXXXXX053 1703703802    1480600104    1709243002   
##  [266] 1100402002    1702304806    1750300404    1100400118    16102003XX   
##  [271] 1781103001    1709253501    699XXXXXXX    3161000117    1702905102   
##  [276] 2290100115    2280100103    1700116701    11004002XX    1480600106   
##  [281] 1709637301    3070300113    1480501702    2311100401    1780200301   
##  [286] 1760801502    1760801002    1480601401    689XXXXXXX    2280101606   
##  [291] 17809XXXXX    307XXXXXXX    1703710801    1120300101    2280100112   
##  [296] 2290100203    1702300415    1080400701    14703004XX    61841007XX   
##  [301] 1750102406    1100400212    23020XXXXX    10901010XX    1780202501   
##  [306] 1750102603    17503XXXXX    22942005XX    1620100402    17070305XX   
##  [311] 1100400203    1750101503    14701XXXXX    1709446601    1610500202   
##  [316] 1090100203    1704700302    22801001XX    170XXXXXXX    3162400201   
##  [321] 1610201201    1760800101    1702905101    18302053XX    10902004XX   
##  [326] 31608XXXXX    694XXXXXXX    693XXXXXXX    17501015XX    17015XXXXX   
##  [331] 1750500901    1703620904    1703919115    1760801004    12314007XX   
##  [336] 22915XXXXX    1620400201    1750102608    1520100103    2290100207   
##  [341] 1620400102    1709448001    691XXXXXXX    1709243003    17092448XX   
##  [346] 1709201911    17071XXXXX    1709447001    17608010XX    2280100128   
##  [351] 1702301127    17063XXXXX    2311005001    1700204220    2312400101   
##  [356] 17032027XX    17405206XX    1210504001    1702222101    2291500501   
##  [361] 1700420101    1410200606    17041XXXXX    1702315101    1703933001   
##  [366] 2280100120    1703922406    1760401201    1830101805    1704200101   
##  [371] 13116XXXXX    1703202702    1220200101    17046036XX    1771000104   
##  [376] 1703600201    3210200229    1703817215    17023231XX    10802XXXXX   
##  [381] 1703911802    1702300101    1700204284    1703817216    1703930001   
##  [386] 1703620708    1703817202    17407001XX    11005XXXXX    1703620901   
##  [391] 1702323101    17033XXXXX    1702317901    1703620702    1703933006   
##  [396] 1706600704    1311600102    1210503801    1707700601    14704XXXXX   
##  [401] 1480400202    1830200201    1210500105    1702300401    17801001XX   
##  [406] 1710200101    1100400105    1830506401    1830202405    1100400110   
##  [411] 10901XXXXX040 2310901006    1830200405    3160800309    1780207001   
##  [416] 199XXXXXXX007 1480401001    1830204504    1480400501    199XXXXXXX008
##  [421] 1480401502    1480403203    11004001XX    1780200403    1480401501   
##  [426] 1100400106    1100400109    1100400104    1100400103    1704100701   
##  [431] 1100400102    1780200302    3070800101    1830201102    1705013202   
##  [436] 1480500408    1750102404    16203XXXXX    1702300405    14806XXXXX   
##  [441] 14805004XX    1750300906    1060800201    1702304605    1702309003   
##  [446] 1703817204    1703716607    1300100501    1210503002    1707700109   
##  [451] 10803XXXXX    1707700302    19002XXXXX    1707700401    17023043XX   
##  [456] 17405XXXXX    1290100304    17036030XX    1702304431    1700204001   
##  [461] 1750102604    1700211502    1703202728    1703202733    1702304814   
##  [466] 1700204225    1080201703    3160400106    1703202801    1703701601   
##  [471] 1702304604    2280102201    1210504202    1470300406    1703639501   
##  [476] 1702329101    1060600603    17011026XX    1703731402    1950100107   
##  [481] 1702304426    1930100101    17002040XX    1210501224    1100100509   
##  [486] 31607008XX    15802XXXXX    2311101205    1703701618    1704603701   
##  [491] 17023044XX    1703701606    1750101506    17037039XX    1290100302   
##  [496] 14306XXXXX    1080201011    17023047XX    1700204228    2280100119   
##  [501] 11002XXXXX    12105033XX    1080300506    31610XXXXX    1750101515   
##  [506] 1703716603    1080201017    1620100401    1703701607    17001025XX   
##  [511] 1703202722    2280100108    2314300113    1703701632    1290200401   
##  [516] 17016XXXXX    3161102502    1703402901    17037016XX    1750101403   
##  [521] 14309011XX    1210501106    1830201401    1760301104    1210502403   
##  [526] 1702700301    1780100112    1170100501    1630200301    1703900801   
##  [531] 1210501107    17603XXXXX    14805004XX034 1230400201    1703910701   
##  [536] 1100500326    1170100102    1950100106    1750100104    199XXXXXXX009
##  [541] 1650100102    1650101203    17506XXXXX    23020018XX    1580200105   
##  [546] 1650101206    3210400105    1709244002    1702300408    1480500407   
##  [551] 2280400203    3210501001    1711501202    1702300406    1210501102   
##  [556] 1480400802    1704100702    17608XXXXX    1780800401    1703935301   
##  [561] 30702018XX    1480500402    1480500404    1760300901    1650100128   
##  [566] 1480500405    18303057XX    1170100105    1170100104    11701XXXXX   
##  [571] 2302007002    17033184XX    18304XXXXX    1830202404    1703745705   
##  [576] 1703745702    18301XXXXX    1703620919    2281200301    30703001XX   
##  [581] 1480401601    1950100103    3160700803    2294200701    3160801404   
##  [586] 1210501103    12305015XX    1830200801    2310901003    1230100401   
##  [591] 1630201302    3161202001    1480401302    3161000105    3161103702   
##  [596] 12301010XX    1230100907    1230100903    316XXXXXXX    1230100908   
##  [601] 2310901004    1230401201    1480400212    1830200501    3160803603   
##  [606] 1780700501    1782000301    649XXXXXXX    3161107501    3160904501   
##  [611] 1480400211    3160700801    3161808901    1830200202    1210500107   
##  [616] 1780100109    22804002XX    30701001XX    1230100902    2312114501   
##  [621] 1230400303    1230100909    1480600401    123XXXXXXX    3161700601   
##  [626] 17204002XX    69302004XX    1230100906    3161202002    1700600602   
##  [631] 1230400802    1480400101    30709003XX    1480400803    1830205003   
##  [636] 17102001XX    1950100101    1706300501    3160700205    3161102001   
##  [641] 3160800105    1830300704    1080100301    2312100501    3070300114   
##  [646] 1100400112    1706902102    1210600208    1210507801    1470200201   
##  [651] 1580200103    2301900301    1703616301    17096373XX    2301900102   
##  [656] 3160700203    1480600512    2282907303    3161000103    2280400501   
##  [661] 1720928802    6930401701    3163900103    1630200201    3161002601   
##  [666] 1703756501    23019XXXXX    17037038XX    11203XXXXX    3070202301   
##  [671] 3161102401    2130301201    2311001001    1700505802    2290100202   
##  [676] 3210502301    3162403901    2310901010    1781602205    1210506702   
##  [681] 1720900501    3160803003    1707027003    1703701604    1580200102   
##  [686] 14315XXXXX    1210501303    1702700302    3161105503    31612040XX   
##  [691] 2280700903    23111003XX    1430901102    1211200103    19009004XX   
##  [696] 1704614110    2280100116    2311100404    1210600202    1702300403   
##  [701] 1210501301    1703730304    1703702801    16204XXXXX    1720400204   
##  [706] 1705700701    1950100105    1703756101    17603009XX    1650100204   
##  [711] 2280104302    22501XXXXX    1703730305    17041251XX    17038155XX   
##  [716] 1760802001    1210502902    12106050XX    1700204010    2290100116   
##  [721] 2282907201    1650100112    3160700802    17046XXXXX    1210601503   
##  [726] 2280102202    1750101509    1210504201    1480500414    1700204222   
##  [731] 2280100129    1703202720    17501014XX    1580200502    1210501217   
##  [736] 1620400701    1750102602    1250300801    1600200101    31604001XX   
##  [741] 2280100123    22801XXXXX027 2280100104    1703903302    1780100901   
##  [746] 1704007501    1706300506    1700206101    1100700801    1703903305   
##  [751] 1771000101    3210400109    14804006XX    3160800311    32109024XX   
##  [756] 1430600701    1630200302    3160400103    1704007502    1780100902   
##  [761] 1703903307    3161100601    1080400713    2250100102    1721335202   
##  [766] 3161102701    1703906010    2311101202    1320801601    2280100105   
##  [771] 1700240505    17039034XX    17505XXXXX    1830804606    1750102301   
##  [776] 1510200104    1703700921    1700204201    2291504101    1750500701   
##  [781] 1700206102    1706505506    1900200505    1700204202    1480400502   
##  [786] 1100400101    1620300201    1400200201    3162300203    1400201601   
##  [791] 1711500401    6930401401    1230400301    1231200102    1400200102   
##  [796] 1780100101    1480403201    1120100301    1400201801    1230100402   
##  [801] 3161202005    1400200701    1500100101    1750101508    1070300901   
##  [806] 1703910202    31604071XX    2280100110    1080400707    1101001505   
##  [811] 1100800702    1210602005    1100500320    2310300302    1771000108   
##  [816] 1090300406    1702326802    1510200101    1740526702    1703402902   
##  [821] 1060600602    1702304609    1210503101    1702304303    1080300501   
##  [826] 1750100102    2280100301    31611XXXXX    2280100111    1210600703   
##  [831] 1704603611    1700204219    1703910501    1703620703    14701013XX   
##  [836] 2250100402    1703817207    1703817226    1700634501    1700255701   
##  [841] 2301900101    1830700101    17035XXXXX    23111004XX    22901XXXXX   
##  [846] 2280400205    1090300401    1231400701    1090101901    1750601201   
##  [851] 1701300701    14002XXXXX    1090100201    1090101602    1510300401   
##  [856] 1400202001    1090101601    1020100201    1480600103    1400201901   
##  [861] 1020100101    1480402801    1400209701    1400233301    1709100301   
##  [866] 3210603301    1709441702    1750102701    2294200901    2301900206   
##  [871] 1520400203    3210505801    1090100803    14802XXXXX    1250203001   
##  [876] 1210507201    17012XXXXX    2311114001    17035169XX    1210504901   
##  [881] 1231200101    1210501104    1100400168    22802XXXXX    1090101403   
##  [886] 1060100301    1090101401    1080100104    3210501002    3210400112   
##  [891] 10801XXXXX    31623XXXXX    3161300102    1480201001    3070100101   
##  [896] 2281201806    31615002XX    1480400602    21302007XX    2311109001   
##  [901] 1090100801    3161102002    1090101801    1090101702    2280100109   
##  [906] 1090100202    1090100701    1100400111    1100400137    1080100106   
##  [911] 30702002XX    1080100302    1100500316    2290100802    3161100301   
##  [916] 11201004XX    1706008301    1830300708    12105011XX    1100400107   
##  [921] 31616003XX    2290100806    1780108003    11005003XX    1780200304   
##  [926] 31642002XX    18303081XX    1650100202    1060600601    11101002XX   
##  [931] 1210501105    2311119501    1830301701    1100400125    1650100122   
##  [936] 2280100133    1211200302    1750500501    1750501701    1702700404   
##  [941] 2290100208    2280101902    2280100131    17059051XX    1703745701   
##  [946] 1703620705    1210505901    1750100207    17030XXXXX    1480201401   
##  [951] 2294200503    1230501504    1780701403    1702309901    1709201909   
##  [956] 15802001XX    2314300109    17115024XX    14002001XX    1230501503   
##  [961] 2282907301    18305003XX    1480401901    1710200103    1709201910   
##  [966] 1210503104    1709201904    1703906011    1230101005    1210602011   
##  [971] 1702313401    1701735702    1080201003    1080201012    2281200302   
##  [976] 112XXXXXXX    1311100701    1780100102    1100400181    1701916502   
##  [981] 1210501204    1750101504    1320801715    1750101505    1480301801   
##  [986] 12111002XX    1210501223    3160407101    1750102303    1210502301   
##  [991] 1700211519    3070400603    1707700201    17000XXXXX    1210501203   
##  [996] 1771000107    1311606801    3161003202    31611017XX    1700204211   
## [1001] 1700212501    1706311703    17032217XX    1700220804    10608XXXXX   
## [1006] 11008XXXXX    22801016XX    1771000103    1210502901    1701102605   
## [1011] 1750101401    1701523304    1210500503    17036207XX    17004089XX   
## [1016] 1210503804    1100100401    1702342201    1780901801    1703756105   
## [1021] 1650104302    1100100402    1702304405    1706505609    1702304721   
## [1026] 1703318404    1703620923    1703202707    1650101217    1703202713   
## [1031] 1700204257    1320802403    1080201031    1705013201    1703718603   
## [1036] 1211100202    1702323102    1480602102    1701640002    3210902401   
## [1041] 14804005XX    22812XXXXX046 3161600503    1100400131    1090500602   
## [1046] 3161600505    12312001XX    1311606804    1780200307    1100100524   
## [1051] 1703318416    109XXXXXXX    1750300901    1706307901    1702305001   
## [1056] 31616XXXXX    1732100401    18305XXXXX    17212XXXXX    1830200408   
## [1061] 1610200301    2302012303    1830804601    1230502901    1210506001   
## [1066] 1830201404    3070300109    2302012304    17603011XX    3160400508   
## [1071] 3070500202    3161200102    1750101512    3210505803    1470401002   
## [1076] 3161101701    1727210301    1702304307    6941400401    1700829701   
## [1081] 1830200802    2290100101    1230100905    3210500301    1780701402   
## [1086] 12301009XX    1470200701    1703922001    2302001801    2312114503   
## [1091] 1830204301    1705700703    23121145XX    1707030504    1620400702   
## [1096] 3210505901    2314300102    1100500301    1750500201    1830202402   
## [1101] 3160806607    1709400101    19501XXXXX    1703001001    1210505803   
## [1106] 10303XXXXX    1703729805    1470300311    1210501222    3161000101   
## [1111] 1500500901    1708800101    2280101607    1900901001    1760800309   
## [1116] 1772301101    1431300105    1830205001    1703933005    1732101702   
## [1121] 17813XXXXX    1781301203    17042XXXXX    1703619705    1400211501   
## [1126] 1703620918    1480600802    1400210001    1702352601    121XXXXXXX019
## [1131] 14106064XX    1211200303    1702304308    1290200402    17033230XX   
## [1136] 22807XXXXX    1750101202    1311601003    10901007XX    1782400201   
## [1141] 2280203001    2280403201    1050200301    22802031XX    1080600302   
## [1146] 1080702001    1100100507    1100702406    1080204002    3074200401   
## [1151] 2280202101    1101001501    17002405XX    1750101507    1080204001   
## [1156] 1650101001    1210501302    1210600207    6180200101    1700102510   
## [1161] 1703202727    1703703908    1706316401    1703235901    3210900519   
## [1166] 2280100101    1703202725    2280100122    1701600103    3210400103   
## [1171] 3160803004    1210503307    1210506302    1703202757    1702304903   
## [1176] 1100500302    1703202729    1703701609    1703700501    1703222501   
## [1181] 1703603010    1650100121    18306XXXXX    1700204232    1060800701   
## [1186] 1930100601    17006345XX    17212010XX    23143001XX    2294200502   
## [1191] 2280700901    1120300103    2290100201    1210600212    17039277XX   
## [1196] 6910500101    3210505802    1210501309    1510402101    1431804501   
## [1201] 1705700602    1030300603    1750101801    1510302204    1120100411   
## [1206] 15204002XX    3160806701    16201XXXXX    1080103402    17506002XX   
## [1211] 1705700402    1721348401    1480201901    14806004XX    1620101102   
## [1216] 1520500101    1900300601    1720919602    3160700220    2294200504   
## [1221] 1100403001    3160800313    1100402001    1700207201    1700200901   
## [1226] 2290100206    2302012302    1703028602    1780200803    31611041XX   
## [1231] 1120100302    1080800301    16105XXXXX    1830201801    1080400709   
## [1236] 1780202502    3161100704    1480603001    1900901801    1760801005   
## [1241] 2312100102    2280400508    1430902102    2090100101    1706338702   
## [1246] 1706306901    31610028XX    1710200102    18302XXXXX    1706306501   
## [1251] 1480402507    1090101001    1211100201    22801019XX    3070800603   
## [1256] 1100100510    1703719402    17095XXXXX    1702632701    17406330XX   
## [1261] 3160700811    1480404101    10201XXXXX    1780100103    1780100104   
## [1266] 1090500601    3161100303    2130500101    3164000501    1950100102   
## [1271] 1700206113    1702300402    1090600901    1100400186    17002061XX   
## [1276] 1060403601    1830500302    1760800302    2280100125    1750601202   
## [1281] 1060800203    1090100804    1610500201    3210902402    1780101902   
## [1286] 1706306601    17813012XX    2280101601    1610200302    17213352XX   
## [1291] 1080400715    1610100102    19301XXXXX    3210400113    1700204229   
## [1296] 1703202717    1703214001    1703202734    1700204009    1470100801   
## [1301] 1702311412    1703919101    1732116201    1300100601    1410204208   
## [1306] 2302001802    2302001803    2280400201    2302007005    2312300101   
## [1311] 2280400208    2280400207    1480400604    2280403702    1480401201   
## [1316] 2302012306    14002018XX    1480401202    3211400102    22823006XX   
## [1321] 1780107901    1780800101    12304XXXXX030 31612020XX    1700204233   
## [1326] 2290100205    1470300413    1703202724    1706300801    1700204240   
## [1331] 1702304411    1703323010    1703620707    1706505611    1702304408   
## [1336] 1740200802    1700204274    1700204255    1700204269    1700204235   
## [1341] 1703202750    1700211511    1703202753    1706543701    1470101308   
## [1346] 1703202745    1703818001    1703926601    1702323104    1703817223   
## [1351] 1702311402    1700211504    1700200101    1700220805    1703209801   
## [1356] 1611101102    1100503407    1704603612    1702304701    1740200416   
## [1361] 1780900302    1700220802    1700204285    1703817206    1703202742   
## [1366] 1700204277    1700204273    1700211509    1703817225    1702311408   
## [1371] 1704118101    1702300201    1703620704    1510200103    1700204234   
## [1376] 1704603613    30742004XX    6940100303    6941400201    6940100302   
## [1381] 6940100305    23020123XX    1703912601    1760600104    17039264XX   
## [1386] 1080201016    23020070XX    1830305702    2290100805    3070300111   
## [1391] 1703920301    1080402304    1703927701    1703922401    1431300104   
## [1396] 1100801007    2282907302    1310702201    1100400183    1520400202   
## [1401] 2314308802    1900200612    1750300907    1480400504    3161202006   
## [1406] 1703933004    1703755301    1703202716    1703318405    2291500104   
## [1411] 1080402901    2280100130    1703817217    3210400111    2290100106   
## [1416] 3210400702    1702342501    225XXXXXXX    1703210102    1830700102   
## [1421] 1080201018    1709243004    13111008XX    1709446602    2281201801   
## [1426] 1620100801    1400201602    32114XXXXX    1620101101    1610501301   
## [1431] 1400204301    1709244801    1320803002    13112XXXXX    1709400901   
## [1436] 1480200601    32109001XX    1100400216    1080103004    1090101005   
## [1441] 1320803201    1100400188    1120200101    1210501801    3161600502   
## [1446] 3160803002    1703703804    1701600104    1470101208    19002006XX   
## [1451] 1702304815    1760801504    3162300402    1760801503    1780100132   
## [1456] 1700208102    1080201009    3160407116    2294900103    1780100110   
## [1461] 1080300509    2020200101    1781303001    3160803001    1830804603   
## [1466] 1706333102    1780100115    1780100116    1100702402    22823XXXXX   
## [1471] 1706325002    1830205902    1780100147    1830204503    1080400703   
## [1476] 1830200415    1080201025    1702304703    1830804607    1700204004   
## [1481] 22959001XX    1700522002    2314300114    1703210001    1170100115   
## [1486] 1701600102    1210502404    1210501108    6560100102    2310901007   
## [1491] 1780702902    1080202701    1702304811    1100400160    1702304610   
## [1496] 1703703907    2280400204    1480500412    3160701102    1704700303   
## [1501] 31612005XX    3161105502    1760301105    3161600701    2310901008   
## [1506] 2282802806    1830204901    1830800902    1480401301    1830201001   
## [1511] 1703603201    1703936701    2314300112    1703741101    1830201103   
## [1516] 3210603305    1700204221    2282802801    2282900603    1703701605   
## [1521] 1080201001    1700204006    1703910201    1700204230    1700204223   
## [1526] 1080201021    1780100122    1703736301    2280400202    1120100401   
## [1531] 1703701614    1830204804    1706324301    1700204236    3160806601   
## [1536] 1703714701    1703603001    1700600603    1170100109    1703710803   
## [1541] 1830506402    1700204224    1210502402    3072300201    1210602003   
## [1546] 3161102104    2280100132    1210601501    3161003201    2290119701   
## [1551] 2290100113   
## 1553 Levels: 1020100101 1020100201 10201XXXXX 1030300603 ... 699XXXXXXX
```

6. Which country had the largest overall catch in the year 2000?

China had the largest overall catch in the year 2000.

```r
largest_catch <- fisheries_tidy %>%
  filter(year == 2000) %>%
  select(country, catch) %>%
  group_by(country) %>%
    arrange(desc(catch))
largest_catch
```

```
## # A tibble: 8,793 x 2
## # Groups:   country [193]
##    country                  catch
##    <fct>                    <dbl>
##  1 China                     9068
##  2 Peru                      5717
##  3 Russian Federation        5065
##  4 Viet Nam                  4945
##  5 Chile                     4299
##  6 China                     3288
##  7 China                     2782
##  8 United States of America  2438
##  9 China                     1234
## 10 Philippines                999
## # ... with 8,783 more rows
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

Morocco caught the most sardines between the years 1990 and 2000.

```r
most_sardines <- fisheries_tidy %>%
  filter(between(year, 1990, 2000), asfis_species_name == 'Sardina pilchardus') %>%
  group_by(country) %>%
  summarize(sum = sum(catch), total = n()) %>%
  arrange(desc(sum))
head(most_sardines)
```

```
## # A tibble: 6 x 3
##   country              sum total
##   <fct>              <dbl> <int>
## 1 Morocco             7470    22
## 2 Spain               3507    33
## 3 Russian Federation  1639    11
## 4 Ukraine             1030    11
## 5 Portugal             818    22
## 6 Greece               528    11
```

8. Which five countries caught the most cephalopods between 2008-2012?

Peru, Thailand, India, Palestine, and Malta caught the most cephalopods between 2008 and 2012.

```r
most_cephalopods <- fisheries_tidy %>%
  filter(between(year, 2008, 2012), isscaap_taxonomic_group == 'Squids, cuttlefishes, octopuses') %>%
  group_by(country) %>%
  summarize(sum = sum(catch), total = n()) %>%
  arrange(desc(sum))
head(most_cephalopods)
```

```
## # A tibble: 6 x 3
##   country                   sum total
##   <fct>                   <dbl> <int>
## 1 Peru                     3422    15
## 2 Thailand                  949    40
## 3 India                     570    10
## 4 Palestine, Occupied Tr.   412    10
## 5 Malta                     398    21
## 6 Singapore                 363    10
```

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

Theragra chalcogramma had the highest catch total between 2008 and 2012.

```r
highest_catch_total <- fisheries_tidy %>%
  filter(between(year, 2008, 2012)) %>%
  group_by(asfis_species_name) %>%
  summarize(sum = sum(catch), total = n()) %>%
  arrange(desc(sum))

head(highest_catch_total)
```

```
## # A tibble: 6 x 3
##   asfis_species_name      sum total
##   <chr>                 <dbl> <int>
## 1 Theragra chalcogramma 41075    35
## 2 Engraulis ringens     35523    14
## 3 Cololabis saira        5733    20
## 4 Sardinella longiceps   3849    30
## 5 Sardinops caeruleus    3204    15
## 6 Brevoortia patronus    3179     5
```

10. Use the data to do at least one analysis of your choice.


```r
fisheries_tidy %>%
  group_by(isscaap_taxonomic_group) %>%
  summarize(number_species = n_distinct(asfis_species_name), total = n()) %>%
  arrange(desc(number_species))
```

```
## # A tibble: 30 x 3
##    isscaap_taxonomic_group       number_species total
##    <chr>                                  <int> <int>
##  1 Miscellaneous coastal fishes             412 69821
##  2 Miscellaneous demersal fishes            185 27360
##  3 Sharks, rays, chimaeras                  171 23210
##  4 Miscellaneous pelagic fishes             127 38992
##  5 Shrimps, prawns                           75 13741
##  6 Cods, hakes, haddocks                     73 21616
##  7 Flounders, halibuts, soles                58 18872
##  8 Clams, cockles, arkshells                 56  5378
##  9 Herrings, sardines, anchovies             55 17867
## 10 Tunas, bonitos, billfishes                47 61839
## # ... with 20 more rows
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
