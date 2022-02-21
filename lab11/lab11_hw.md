---
title: "Lab 11 Homework"
author: "Evan Roybal"
date: "2022-02-21"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(skimr)
library(ggthemes)

options(scipen = 999)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
#install.packages("gapminder")
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  


```r
head(gapminder)
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

```r
dim(gapminder)
```

```
## [1] 1704    6
```

```r
colnames(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```

```r
skim(gapminder)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |gapminder |
|Number of rows           |1704      |
|Number of columns        |6         |
|_______________________  |          |
|Column type frequency:   |          |
|factor                   |2         |
|numeric                  |4         |
|________________________ |          |
|Group variables          |None      |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts                             |
|:-------------|---------:|-------------:|:-------|--------:|:--------------------------------------|
|country       |         0|             1|FALSE   |      142|Afg: 12, Alb: 12, Alg: 12, Ang: 12     |
|continent     |         0|             1|FALSE   |        5|Afr: 624, Asi: 396, Eur: 360, Ame: 300 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|        mean|           sd|       p0|        p25|        p50|         p75|         p100|hist                                     |
|:-------------|---------:|-------------:|-----------:|------------:|--------:|----------:|----------:|-----------:|------------:|:----------------------------------------|
|year          |         0|             1|     1979.50|        17.27|  1952.00|    1965.75|    1979.50|     1993.25|       2007.0|▇▅▅▅▇ |
|lifeExp       |         0|             1|       59.47|        12.92|    23.60|      48.20|      60.71|       70.85|         82.6|▁▆▇▇▇ |
|pop           |         0|             1| 29601212.32| 106157896.74| 60011.00| 2793664.00| 7023595.50| 19585221.75| 1318683096.0|▇▁▁▁▁ |
|gdpPercap     |         0|             1|     7215.33|      9857.45|   241.17|    1202.06|    3531.85|     9325.46|     113523.1|▇▁▁▁▁ |

```r
anyNA(gapminder)
```

```
## [1] FALSE
```

```r
miss_var_summary(gapminder)
```

```
## # A tibble: 6 x 3
##   variable  n_miss pct_miss
##   <chr>      <int>    <dbl>
## 1 country        0        0
## 2 continent      0        0
## 3 year           0        0
## 4 lifeExp        0        0
## 5 pop            0        0
## 6 gdpPercap      0        0
```


**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**


```r
gapminder %>%
  group_by(year) %>%
  summarise(average_life_exp = mean(lifeExp)) %>%
  ggplot(aes(x = year, y = average_life_exp)) +
  geom_line() +
  labs(title = 'Average Life Expectancy Between 1952 and 2007',
       y = 'Average Life Expectancy Globally') +
  theme_gdocs()
```

![](lab11_hw_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**


```r
gapminder %>%
  filter(year == 1952 | year == 2007) %>%
  group_by(year) %>%
  ggplot(aes(x = year, y = lifeExp, group = year)) +
  geom_boxplot() +
  theme_solarized() +
  labs(title = 'Life Expectancy Distributions for Years 1952 and 2007',
       x = 'year',
       y = 'Life Expectancy')
```

![](lab11_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**


```r
continent_life_exp <- gapminder %>%
  group_by(continent) %>%
  summarise(min_lifeExp = min(lifeExp), max_lifeExp = max(lifeExp), mean_lifeExp = mean(lifeExp))

continent_life_exp
```

```
## # A tibble: 5 x 4
##   continent min_lifeExp max_lifeExp mean_lifeExp
##   <fct>           <dbl>       <dbl>        <dbl>
## 1 Africa           23.6        76.4         48.9
## 2 Americas         37.6        80.7         64.7
## 3 Asia             28.8        82.6         60.1
## 4 Europe           43.6        81.8         71.9
## 5 Oceania          69.1        81.2         74.3
```


**5. How has life expectancy changed between 1952-2007 for each continent?**


```r
gapminder %>%
  group_by(continent, year) %>%
  summarise(average_lifeExp = mean(lifeExp)) %>%
  ggplot(aes(x = year, y = average_lifeExp, color = continent)) +
  geom_line() +
  geom_point(shape = 'triangle') +
  theme_light() +
  labs(title = 'Average Life Expectancy For Continents From 1952 to 2007',
       y = 'Average Life Expectancy')
```

```
## `summarise()` has grouped output by 'continent'. You can override using the `.groups` argument.
```

![](lab11_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**


```r
gapminder %>%
  ggplot(aes(x = gdpPercap, y = lifeExp)) +
  geom_point(alpha = .75, size = 1) +
  scale_x_log10() +
  geom_smooth(method = lm) +
  theme_clean() +
  labs(title = 'GDP Per Capita Compared to Life Expectancy',
       x = 'GDP Per Capita',
       y = 'Life Expectancy (in years)')
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


**7. Which countries have had the largest population growth since 1952?**


```r
gapminder_year_wide <- gapminder %>%
  select(country, pop, year) %>%
  filter(year == 1952 | year == 2007) %>%
  pivot_wider(names_from = year,
              values_from = pop)
gapminder_year_wide <- gapminder_year_wide %>%
  mutate(pop_change = gapminder_year_wide$`2007` - gapminder_year_wide$`1952`) %>%
  arrange(desc(pop_change))

gapminder_year_wide
```

```
## # A tibble: 142 x 4
##    country          `1952`     `2007` pop_change
##    <fct>             <int>      <int>      <int>
##  1 China         556263527 1318683096  762419569
##  2 India         372000000 1110396331  738396331
##  3 United States 157553000  301139947  143586947
##  4 Indonesia      82052000  223547000  141495000
##  5 Brazil         56602560  190010647  133408087
##  6 Pakistan       41346560  169270617  127924057
##  7 Bangladesh     46886859  150448339  103561480
##  8 Nigeria        33119096  135031164  101912068
##  9 Mexico         30144317  108700891   78556574
## 10 Philippines    22438691   91077287   68638596
## # ... with 132 more rows
```

```r
nrow(gapminder_year_wide)
```

```
## [1] 142
```

```r
gapminder_year_wide %>%
  top_n(10) %>%
  ggplot(aes(x = reorder(country, pop_change), y = pop_change)) +
  geom_col() +
  labs(title = 'Top 10 Population Changes in Countries From 1952 to 2007',
       x = '',
       y = 'Population Change') +
  theme_clean() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

```
## Selecting by pop_change
```

![](lab11_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
#top five = China, India, US, Indonesia, Brazil
gapminder %>%
  select(country, pop, year) %>%
  filter(country == 'China' | country == 'India' | country == 'United States' | country == 'Indonesia' | country == 'Brazil') %>%
  ggplot(aes(x = year, y = pop)) +
  geom_point() +
  geom_line() +
  facet_wrap(vars(country)) +
  theme_gray() +
  labs(title = 'Top 5 Countries for Population Growth from 1952 to 2007',
       x = '',
       y = 'Population Size')
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**9. How does per-capita GDP growth compare between these same five countries?**


```r
gapminder %>%
  select(country, gdpPercap, year) %>%
  filter(country == 'China' | country == 'India' | country == 'United States' | country == 'Indonesia' | country == 'Brazil') %>%
  ggplot(aes(x = year, y = gdpPercap)) +
  geom_point() +
  geom_line() +
  facet_wrap(vars(country)) +
  theme_gray() +
  labs(title = 'GDP Per Capita in Top Five Countries for Population Growth From 1952 to 2007',
       x = '',
       y = 'log10 of GDP Per Capita') +
  scale_y_log10()
```

![](lab11_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


**10. Make one plot of your choice that uses faceting!**


```r
gapminder %>%
  select(country, gdpPercap, lifeExp) %>%
  filter(country == 'China' | country == 'India' | country == 'United States' | country == 'Indonesia' | country == 'Brazil') %>%
  ggplot(aes(x = lifeExp, y = gdpPercap)) +
  geom_point() +
  geom_line() +
  facet_wrap(vars(country)) +
  theme_gray() +
  labs(title = 'Comparison of GDP Per Capita and Life Expectancy for Top Five Countries for Populuation Growth',
       x = 'Life Expectancy (in years)',
       y = 'log10 of GDP per Capita') +
  scale_y_log10()
```

![](lab11_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
