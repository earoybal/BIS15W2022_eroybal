---
title: "Lab 14 HW"
output: 
  html_document: 
    keep_md: yes
author: "Evan Roybal"
date: '2022-02-28'
keep_md: yes
---


### Load the Libraries

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.8
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
library("palmerpenguins")
library(skimr)

#install.packages("ggVennDiagram")
library(ggVennDiagram)
library(RColorBrewer)

#install.packages("ggworldcloud")
library(ggwordcloud)


options(scipen=999) #cancels the use of scientific notation for the session
```


### Data

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
beachbugs_long <- readr::read_csv("data/beachbugs_long.csv")
```

```
## Rows: 66 Columns: 3
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (1): site
## dbl (2): year, buglevels
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

### 1. 
Clean up the column names (no capitals, not spaces) of `superhero_info`, then use 2 functions to remind yourself of structure of the `superhero_info` data set.


```r
superhero_info_clean <- superhero_info %>% clean_names()
colnames(superhero_info_clean)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

```r
skim(superhero_info_clean)
```


Table: Data summary

|                         |                     |
|:------------------------|:--------------------|
|Name                     |superhero_info_clean |
|Number of rows           |734                  |
|Number of columns        |10                   |
|_______________________  |                     |
|Column type frequency:   |                     |
|character                |8                    |
|numeric                  |2                    |
|________________________ |                     |
|Group variables          |None                 |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|name          |         0|          1.00|   1|  25|     0|      715|          0|
|gender        |        29|          0.96|   4|   6|     0|        2|          0|
|eye_color     |       172|          0.77|   3|  23|     0|       22|          0|
|race          |       304|          0.59|   5|  18|     0|       61|          0|
|hair_color    |       172|          0.77|   3|  16|     0|       29|          0|
|publisher     |        15|          0.98|   4|  17|     0|       24|          0|
|skin_color    |       662|          0.10|   3|  14|     0|       16|          0|
|alignment     |         7|          0.99|   3|   7|     0|        3|          0|


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|     sd|   p0| p25| p50| p75| p100|hist                                     |
|:-------------|---------:|-------------:|------:|------:|----:|---:|---:|---:|----:|:----------------------------------------|
|height        |       217|          0.70| 186.73|  59.25| 15.2| 173| 183| 191|  975|??????????????? |
|weight        |       239|          0.67| 112.25| 104.15|  2.0|  61|  81| 108|  900|??????????????? |

```r
head(superhero_info_clean)
```

```
## # A tibble: 6 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 A-Bomb Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
## 2 Abe S~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
## 3 Abin ~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
## 4 Abomi~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
## 5 Abrax~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
## 6 Absor~ Male   blue      Human No Hair       193 Marvel C~ <NA>       bad      
## # ... with 1 more variable: weight <dbl>
```

### 2.
Are bad guys bigger? Make box-plots of weight by `alignment` (alignment on the x-axis).

```r
superhero_info_clean %>%
  na.exclude() %>%
  ggplot(aes(x = alignment, y = weight)) +
  geom_boxplot() +
  labs(title = 'Weight distribution of good and bad superheroes')
```

![](lab14_hw_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

### 3. 
Now, make a violin plot of weight by `alignment` (alignment on the x-axis). Add some color!
  What information can you observe in the violin plot that was not visible in the boxplot?

```r
superhero_info_clean %>%
  na.exclude() %>%
  ggplot(aes(x = alignment, y = weight, color = alignment)) +
  geom_boxplot(alpha = .3, color = 'grey') +
  geom_violin(alpha = .5)
```

![](lab14_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
  labs(title = 'Weight distribution of good and bad superheroes')
```

```
## $title
## [1] "Weight distribution of good and bad superheroes"
## 
## attr(,"class")
## [1] "labels"
```
There are small spots where the different alignments have a concentration of weights larger than what the boxplot includes in the box.


### 4. 
Use `alpha = .5` in `geom_boxplot()` and `geom_violin()` to layer both plots on top of one another. What does this tell you about the distribution of weight in "`bad`" guys?

```r
superhero_info_clean %>%
  na.exclude() %>%
  ggplot(aes(x = alignment, y = weight, color = alignment)) +
  geom_boxplot(alpha = .3, color = 'grey') +
  geom_violin(alpha = .5)
```

![](lab14_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
  labs(title = 'Weight distribution of good and bad superheroes')
```

```
## $title
## [1] "Weight distribution of good and bad superheroes"
## 
## attr(,"class")
## [1] "labels"
```
Bad guys have these small fluctuations in weight that are much higher than the main body of the box.

### 5. 
Box plots are great for showing how the distribution of a numeric variable (e.g. weight) varies among a categorical variable (e.g. alignment).
  Make your own violin plot with the superhero data. 
  What is your numeric data? 
  What is your categorical variable?


```r
superhero_info_clean %>%
  filter(gender != 'NA') %>%
  ggplot(aes(x = gender, y = height, color = gender)) +
  geom_boxplot(alpha = .3, color = 'grey') +
  geom_violin(alpha = .5) +
  labs(title = 'Distribution of height among the different superhero genders')
```

```
## Warning: Removed 203 rows containing non-finite values (stat_boxplot).
```

```
## Warning: Removed 203 rows containing non-finite values (stat_ydensity).
```

![](lab14_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
numeric is height, categorical is gender.


### 6. 
Remind yourself what `beachbugs` looks like. Then generate a heatmap of buglevels by site and year. 
color it with `scale_fill_gradient(low="yellow", high="red")` or colors of your choice. you may find it looks best with one color!
(dont forget, `coord_flip()` is a quick way to improve the look of your plot if you dont like the default orientation)


```r
head(beachbugs_long)
```

```
## # A tibble: 6 x 3
##    year site               buglevels
##   <dbl> <chr>                  <dbl>
## 1  2013 Bondi Beach            32.2 
## 2  2013 Bronte Beach           26.8 
## 3  2013 Clovelly Beach          9.28
## 4  2013 Coogee Beach           39.7 
## 5  2013 Gordons Bay (East)     24.8 
## 6  2013 Little Bay Beach      122.
```

```r
anyNA(beachbugs_long)
```

```
## [1] FALSE
```


```r
beachbugs_long %>%
  ggplot(aes(x = site, y = year, fill = buglevels)) +
  geom_tile() +
  scale_fill_gradient(low = 'yellow', high = 'red') +
  coord_flip() +
  labs(title = 'Prevalence of bugs at different beaches',
       fill = 'bug levels')
```

![](lab14_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

### 7.  
Use the provided code to normalize the beachbug data set. 
Then make a heatmap with the `beachbugs_normalized` data, and use the same color chois as above. Which heatmap do you think is more informative? why?


```r
#makes a new column of the highest buglevel observed at each site
beachbugs_w_max <- beachbugs_long %>% 
  group_by(site) %>% 
  mutate(max_buglevel = max(buglevels, na.rm=T)) %>% 
  arrange(site)
beachbugs_w_max
```

```
## # A tibble: 66 x 4
## # Groups:   site [11]
##     year site         buglevels max_buglevel
##    <dbl> <chr>            <dbl>        <dbl>
##  1  2013 Bondi Beach       32.2         32.2
##  2  2014 Bondi Beach       11.1         32.2
##  3  2015 Bondi Beach       14.3         32.2
##  4  2016 Bondi Beach       19.4         32.2
##  5  2017 Bondi Beach       13.2         32.2
##  6  2018 Bondi Beach       22.9         32.2
##  7  2013 Bronte Beach      26.8         61.3
##  8  2014 Bronte Beach      17.5         61.3
##  9  2015 Bronte Beach      23.6         61.3
## 10  2016 Bronte Beach      61.3         61.3
## # ... with 56 more rows
```

```r
#makes a new table where the buglevels are normalized to the max_buglevel
beachbugs_normalized <- beachbugs_w_max %>% 
  group_by(site) %>% 
  mutate(norm_buglevel = buglevels/max_buglevel) %>% 
  arrange(site,year) %>%
  select(site, year, norm_buglevel)   # you dont have to select(), but I think its a clearer looking table
beachbugs_normalized
```

```
## # A tibble: 66 x 3
## # Groups:   site [11]
##    site          year norm_buglevel
##    <chr>        <dbl>         <dbl>
##  1 Bondi Beach   2013         1    
##  2 Bondi Beach   2014         0.344
##  3 Bondi Beach   2015         0.445
##  4 Bondi Beach   2016         0.601
##  5 Bondi Beach   2017         0.409
##  6 Bondi Beach   2018         0.710
##  7 Bronte Beach  2013         0.436
##  8 Bronte Beach  2014         0.285
##  9 Bronte Beach  2015         0.385
## 10 Bronte Beach  2016         1    
## # ... with 56 more rows
```
 

```r
beachbugs_normalized %>%
  ggplot(aes(x = site, y = year, fill = norm_buglevel)) +
  geom_tile() +
  scale_fill_gradient(low = 'yellow', high = 'red') +
  coord_flip() +
  labs(title = 'normalized bug levels at beaches',
       fill = 'normalized bug level')
```

![](lab14_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
Depending on what you're looking for either heatmap could be more informative. If you're looking for a heatmap that simply shows how many bugs there are at a particular beach then the first one is more informative. If you want to see how the relative bug levels have been different over the years at different beaches then the second map is more informative.


### 8.
Let's make a venn diagram of `superhero_info`, from 4 questions:
Is their alignment evil? 
Are their eyes red?
Are they male?
Are they bald?

Start by making the 4 vectors, then the list of vectors. The vector for alignment is provided:
### super heros venn

```r
# evil

evil_vec <- superhero_info_clean %>%
  filter(alignment == "bad")%>%
  pull(name)

# red eyes

red_eyes_vec <- superhero_info_clean %>%
  filter(eye_color == 'red') %>%
  pull(name)

# male

male_vec <- superhero_info_clean %>%
  filter(gender == 'Male') %>%
  pull(name)

# bald
bald_vec <- superhero_info_clean %>%
  filter(hair_color == 'No Hair') %>%
  pull(name)
```

Your list of vectors will look something like this:

```r
 questions_list <- list(evil_vec, red_eyes_vec, male_vec, bald_vec)
```

### 9. 
Let's make the venn diagram! use the code from lab as a reference. 

```r
# something like:
ggVennDiagram(questions_list, category.names = c("evil", "red eyes", "male", "bald"))
```

![](lab14_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


### 10. Choose one intersection of the venn diagram that is interesting to you. Use dplyr to find the names of the superheros in that intersection. 


```r
superhero_info_clean %>%
  filter(alignment == 'bad',
         eye_color == 'red',
         gender != 'Male')
```

```
## # A tibble: 1 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Spide~ Female red       <NA>  White         178 Marvel C~ <NA>       bad      
## # ... with 1 more variable: weight <dbl>
```



### 11. Make another venn diagram with the `superhero_info` data. What are your questions? ( At least 2!) 

```r
race_vec <- superhero_info_clean %>%
  filter(race == 'Human') %>%
  pull(name)

hair_vec <- superhero_info_clean %>%
  filter(hair_color == 'Black') %>%
  pull(name)

publisher_vec <- superhero_info_clean %>%
  filter(publisher == 'Marvel Comics') %>%
  pull(name)

race_hair_publisher_list <- list(race_vec, hair_vec, publisher_vec)

ggVennDiagram(race_hair_publisher_list, category.names = c('Human', 'Black Hair', 'Marvel Comics')) +
  labs(title = "How many human superheroes with black hair were created by Marvel?")
```

![](lab14_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->



### 12.
What are some very common super powers? Lets make a word cloud with the `superhero_powers` data.

Use the provided code to make the frequency table, then make a word cloud with it. 
Remember, you can change `scale_size_area(max_size = 20)` to a different number if the words wont fit!


```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names  Agility `Accelerated Healing` `Lantern Power Ri~` `Dimensional A~`
##   <chr>       <lgl>   <lgl>                 <lgl>               <lgl>           
## 1 3-D Man     TRUE    FALSE                 FALSE               FALSE           
## 2 A-Bomb      FALSE   TRUE                  FALSE               FALSE           
## 3 Abe Sapien  TRUE    TRUE                  FALSE               FALSE           
## 4 Abin Sur    FALSE   FALSE                 TRUE                FALSE           
## 5 Abomination FALSE   TRUE                  FALSE               FALSE           
## 6 Abraxas     FALSE   FALSE                 FALSE               TRUE            
## # ... with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>,
## #   `Danger Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>,
## #   `Weapons Master` <lgl>, `Power Augmentation` <lgl>,
## #   `Animal Attributes` <lgl>, Longevity <lgl>, Intelligence <lgl>,
## #   `Super Strength` <lgl>, Cryokinesis <lgl>, Telepathy <lgl>,
## #   `Energy Armor` <lgl>, `Energy Blasts` <lgl>, Duplication <lgl>, ...
```

```r
power_frequency <- superhero_powers %>% 
  select(-hero_names) %>%
  summarise_all(~(sum(.))) %>%
  pivot_longer(everything(), names_to = "power", values_to = "freq")
power_frequency
```

```
## # A tibble: 167 x 2
##    power                  freq
##    <chr>                 <int>
##  1 Agility                 242
##  2 Accelerated Healing     178
##  3 Lantern Power Ring       11
##  4 Dimensional Awareness    25
##  5 Cold Resistance          47
##  6 Durability              257
##  7 Stealth                 126
##  8 Energy Absorption        77
##  9 Flight                  212
## 10 Danger Sense             30
## # ... with 157 more rows
```



```r
power_frequency %>%
  ggplot(aes(
    label = power,
    size = freq,
    color = power
  )) +
  geom_text_wordcloud() +
  scale_size_area(max_size = 10) +
  theme_minimal()
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=12, height=8' are unlikely values in pixels

## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=12, height=8' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=16, height=8' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=16, height=12' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=8, height=8' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=16, height=8' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=12, height=8' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=12, height=12' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=16, height=12' are unlikely values in pixels
```

```
## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=16, height=8' are unlikely values in pixels

## Warning in png(filename = tmp_file, width = gw_pix, height = gh_pix, res =
## dev_dpi, : 'width=16, height=8' are unlikely values in pixels
```

![](lab14_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
### 13.  
Who are some very powerful supers? 
 Lets make a different word cloud with the `superhero_powers` data. 
Use the provided code to make the frequency table, then make a word cloud with it.
You can use `hero_names` for the labels, and `sum_powers` for size. 


```r
power_quantity <- superhero_powers %>% 
  pivot_longer(-hero_names, names_to = "power", values_to = "yes_or_no") %>% 
  group_by(hero_names) %>% 
  mutate(sum_powers = sum(yes_or_no, na.rm=T)) %>%
  arrange(desc(sum_powers), hero_names, desc(yes_or_no))
power_quantity
```

```
## # A tibble: 111,389 x 4
## # Groups:   hero_names [667]
##    hero_names power                 yes_or_no sum_powers
##    <chr>      <chr>                 <lgl>          <int>
##  1 Spectre    Agility               TRUE              49
##  2 Spectre    Accelerated Healing   TRUE              49
##  3 Spectre    Dimensional Awareness TRUE              49
##  4 Spectre    Stealth               TRUE              49
##  5 Spectre    Energy Absorption     TRUE              49
##  6 Spectre    Flight                TRUE              49
##  7 Spectre    Marksmanship          TRUE              49
##  8 Spectre    Longevity             TRUE              49
##  9 Spectre    Intelligence          TRUE              49
## 10 Spectre    Super Strength        TRUE              49
## # ... with 111,379 more rows
```

```r
#we have to trim down to only the top 50, or it will crowd the image!
power_quantity <- power_quantity %>% 
  ungroup %>%
  distinct(hero_names, sum_powers) %>%
  slice_max(sum_powers, n = 50)
power_quantity
```

```
## # A tibble: 51 x 2
##    hero_names        sum_powers
##    <chr>                  <int>
##  1 Spectre                   49
##  2 Amazo                     44
##  3 Living Tribunal           35
##  4 Martian Manhunter         35
##  5 Man of Miracles           34
##  6 Captain Marvel            33
##  7 T-X                       33
##  8 Galactus                  32
##  9 T-1000                    32
## 10 Mister Mxyzptlk           31
## # ... with 41 more rows
```




```r
power_quantity %>%
  ggplot(aes(
    label = hero_names,
    size = sum_powers,
    color = hero_names
  )) +
  geom_text_wordcloud() +
  scale_size_area(max_size = 10)
```

![](lab14_hw_files/figure-html/unnamed-chunk-21-1.png)<!-- -->
## That's it! ????
Thanks for coding with us all winter! 
Now go finish your group project :) 

