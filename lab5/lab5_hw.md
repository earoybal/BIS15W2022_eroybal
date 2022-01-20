---
title: "dplyr Superhero"
date: "2022-01-20"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Load the tidyverse

```r
library("tidyverse")
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

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

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
superhero_info <- clean_names(superhero_info)
colnames(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names  Agility `Accelerated Healing` `Lantern Power Ri~ `Dimensional Awa~
##   <chr>       <lgl>   <lgl>                 <lgl>              <lgl>            
## 1 3-D Man     TRUE    FALSE                 FALSE              FALSE            
## 2 A-Bomb      FALSE   TRUE                  FALSE              FALSE            
## 3 Abe Sapien  TRUE    TRUE                  FALSE              FALSE            
## 4 Abin Sur    FALSE   FALSE                 TRUE               FALSE            
## 5 Abomination FALSE   TRUE                  FALSE              FALSE            
## 6 Abraxas     FALSE   FALSE                 FALSE              TRUE             
## # ... with 163 more variables: Cold Resistance <lgl>, Durability <lgl>,
## #   Stealth <lgl>, Energy Absorption <lgl>, Flight <lgl>, Danger Sense <lgl>,
## #   Underwater breathing <lgl>, Marksmanship <lgl>, Weapons Master <lgl>,
## #   Power Augmentation <lgl>, Animal Attributes <lgl>, Longevity <lgl>,
## #   Intelligence <lgl>, Super Strength <lgl>, Cryokinesis <lgl>,
## #   Telepathy <lgl>, Energy Armor <lgl>, Energy Blasts <lgl>,
## #   Duplication <lgl>, Size Changing <lgl>, Density Control <lgl>, ...
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  


```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutral <- superhero_info %>%
  filter(alignment == 'neutral')
unique(neutral$name)
```

```
##  [1] "Bizarro"         "Black Flash"     "Captain Cold"    "Copycat"        
##  [5] "Deadpool"        "Deathstroke"     "Etrigan"         "Galactus"       
##  [9] "Gladiator"       "Indigo"          "Juggernaut"      "Living Tribunal"
## [13] "Lobo"            "Man-Bat"         "One-Above-All"   "Raven"          
## [17] "Red Hood"        "Red Hulk"        "Robin VI"        "Sandman"        
## [21] "Sentry"          "Sinestro"        "The Comedian"    "Toad"
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
reduced_sh_info <- superhero_info %>%
  select(name, alignment, race)
head(reduced_sh_info)
```

```
## # A tibble: 6 x 3
##   name          alignment race             
##   <chr>         <chr>     <chr>            
## 1 A-Bomb        good      Human            
## 2 Abe Sapien    good      Icthyo Sapien    
## 3 Abin Sur      good      Ungaran          
## 4 Abomination   bad       Human / Radiation
## 5 Abraxas       bad       Cosmic Entity    
## 6 Absorbing Man bad       Human
```

## Not Human
4. List all of the superheros that are not human.

```r
not_human <- reduced_sh_info %>%
  filter(race != 'Human')
head(not_human)
```

```
## # A tibble: 6 x 3
##   name        alignment race             
##   <chr>       <chr>     <chr>            
## 1 Abe Sapien  good      Icthyo Sapien    
## 2 Abin Sur    good      Ungaran          
## 3 Abomination bad       Human / Radiation
## 4 Abraxas     bad       Cosmic Entity    
## 5 Ajax        bad       Cyborg           
## 6 Alien       bad       Xenomorph XX121
```

```r
unique(not_human$name)
```

```
##   [1] "Abe Sapien"                "Abin Sur"                 
##   [3] "Abomination"               "Abraxas"                  
##   [5] "Ajax"                      "Alien"                    
##   [7] "Amazo"                     "Angel"                    
##   [9] "Angel Dust"                "Anti-Monitor"             
##  [11] "Anti-Venom"                "Apocalypse"               
##  [13] "Aqualad"                   "Aquaman"                  
##  [15] "Archangel"                 "Ardina"                   
##  [17] "Atlas"                     "Aurora"                   
##  [19] "Azazel"                    "Beast"                    
##  [21] "Beyonder"                  "Big Barda"                
##  [23] "Bill Harken"               "Bionic Woman"             
##  [25] "Birdman"                   "Bishop"                   
##  [27] "Bizarro"                   "Black Bolt"               
##  [29] "Black Canary"              "Black Flash"              
##  [31] "Blackout"                  "Blackwulf"                
##  [33] "Blade"                     "Blink"                    
##  [35] "Bloodhawk"                 "Boba Fett"                
##  [37] "Boom-Boom"                 "Brainiac"                 
##  [39] "Brundlefly"                "Cable"                    
##  [41] "Cameron Hicks"             "Captain Atom"             
##  [43] "Captain Marvel"            "Captain Planet"           
##  [45] "Captain Universe"          "Carnage"                  
##  [47] "Century"                   "Cerebra"                  
##  [49] "Chamber"                   "Colossus"                 
##  [51] "Copycat"                   "Crystal"                  
##  [53] "Cyborg"                    "Cyborg Superman"          
##  [55] "Cyclops"                   "Darkseid"                 
##  [57] "Darkstar"                  "Darth Maul"               
##  [59] "Darth Vader"               "Data"                     
##  [61] "Dazzler"                   "Deadpool"                 
##  [63] "Deathlok"                  "Demogoblin"               
##  [65] "Doc Samson"                "Donatello"                
##  [67] "Donna Troy"                "Doomsday"                 
##  [69] "Dr Manhattan"              "Drax the Destroyer"       
##  [71] "Etrigan"                   "Evil Deadpool"            
##  [73] "Evilhawk"                  "Exodus"                   
##  [75] "Faora"                     "Fin Fang Foom"            
##  [77] "Firestar"                  "Franklin Richards"        
##  [79] "Galactus"                  "Gambit"                   
##  [81] "Gamora"                    "Garbage Man"              
##  [83] "Gary Bell"                 "General Zod"              
##  [85] "Ghost Rider"               "Gladiator"                
##  [87] "Godzilla"                  "Goku"                     
##  [89] "Gorilla Grodd"             "Greedo"                   
##  [91] "Groot"                     "Guy Gardner"              
##  [93] "Havok"                     "Hela"                     
##  [95] "Hellboy"                   "Hercules"                 
##  [97] "Hulk"                      "Human Torch"              
##  [99] "Husk"                      "Hybrid"                   
## [101] "Hyperion"                  "Iceman"                   
## [103] "Indigo"                    "Ink"                      
## [105] "Invisible Woman"           "Jar Jar Binks"            
## [107] "Jean Grey"                 "Jubilee"                  
## [109] "Junkpile"                  "K-2SO"                    
## [111] "Killer Croc"               "Kilowog"                  
## [113] "King Kong"                 "King Shark"               
## [115] "Krypto"                    "Lady Deathstrike"         
## [117] "Legion"                    "Leonardo"                 
## [119] "Living Tribunal"           "Lobo"                     
## [121] "Loki"                      "Magneto"                  
## [123] "Man of Miracles"           "Mantis"                   
## [125] "Martian Manhunter"         "Master Chief"             
## [127] "Medusa"                    "Mera"                     
## [129] "Metallo"                   "Michelangelo"             
## [131] "Mister Fantastic"          "Mister Knife"             
## [133] "Mister Mxyzptlk"           "Mister Sinister"          
## [135] "MODOK"                     "Mogo"                     
## [137] "Mr Immortal"               "Mystique"                 
## [139] "Namor"                     "Nebula"                   
## [141] "Negasonic Teenage Warhead" "Nina Theroux"             
## [143] "Nova"                      "Odin"                     
## [145] "One-Above-All"             "Onslaught"                
## [147] "Parademon"                 "Phoenix"                  
## [149] "Plantman"                  "Polaris"                  
## [151] "Power Girl"                "Power Man"                
## [153] "Predator"                  "Professor X"              
## [155] "Psylocke"                  "Q"                        
## [157] "Quicksilver"               "Rachel Pirzad"            
## [159] "Raphael"                   "Red Hulk"                 
## [161] "Red Tornado"               "Rhino"                    
## [163] "Rocket Raccoon"            "Sabretooth"               
## [165] "Sauron"                    "Scarlet Spider II"        
## [167] "Scarlet Witch"             "Sebastian Shaw"           
## [169] "Sentry"                    "Shadow Lass"              
## [171] "Shadowcat"                 "She-Thing"                
## [173] "Sif"                       "Silver Surfer"            
## [175] "Sinestro"                  "Siren"                    
## [177] "Snake-Eyes"                "Solomon Grundy"           
## [179] "Spawn"                     "Spectre"                  
## [181] "Spider-Carnage"            "Spock"                    
## [183] "Spyke"                     "Star-Lord"                
## [185] "Starfire"                  "Static"                   
## [187] "Steppenwolf"               "Storm"                    
## [189] "Sunspot"                   "Superboy-Prime"           
## [191] "Supergirl"                 "Superman"                 
## [193] "Swamp Thing"               "Swarm"                    
## [195] "T-1000"                    "T-800"                    
## [197] "T-850"                     "T-X"                      
## [199] "Thanos"                    "Thing"                    
## [201] "Thor"                      "Thor Girl"                
## [203] "Toad"                      "Toxin"                    
## [205] "Trigon"                    "Triton"                   
## [207] "Ultron"                    "Utgard-Loki"              
## [209] "Vegeta"                    "Venom"                    
## [211] "Venom III"                 "Venompool"                
## [213] "Vision"                    "Warpath"                  
## [215] "Wolverine"                 "Wonder Girl"              
## [217] "Wonder Woman"              "X-23"                     
## [219] "Ymir"                      "Yoda"
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- superhero_info %>%
  filter(alignment == 'good')
```


```r
bad_guys <- superhero_info %>%
  filter(alignment == 'bad')
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys$race)
```

```
##     good_guys$race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
asgardians <- good_guys %>%
  filter(race == 'Asgardian') %>%
  select(name)
asgardians
```

```
## # A tibble: 3 x 1
##   name     
##   <chr>    
## 1 Sif      
## 2 Thor     
## 3 Thor Girl
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
tall_bad_guys <- bad_guys %>%
  filter(height > 200) %>%
  select(name)
tall_bad_guys
```

```
## # A tibble: 25 x 1
##    name          
##    <chr>         
##  1 Abomination   
##  2 Alien         
##  3 Amazo         
##  4 Apocalypse    
##  5 Bane          
##  6 Bloodaxe      
##  7 Darkseid      
##  8 Doctor Doom   
##  9 Doctor Doom II
## 10 Doomsday      
## # ... with 15 more rows
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
tabyl(good_guys$hair_color)
```

```
##  good_guys$hair_color   n     percent valid_percent
##                Auburn  10 0.020161290   0.026178010
##                 black   3 0.006048387   0.007853403
##                 Black 108 0.217741935   0.282722513
##                 blond   2 0.004032258   0.005235602
##                 Blond  85 0.171370968   0.222513089
##                  Blue   1 0.002016129   0.002617801
##                 Brown  55 0.110887097   0.143979058
##         Brown / Black   1 0.002016129   0.002617801
##         Brown / White   4 0.008064516   0.010471204
##                 Green   7 0.014112903   0.018324607
##                  Grey   2 0.004032258   0.005235602
##                Indigo   1 0.002016129   0.002617801
##               Magenta   1 0.002016129   0.002617801
##               No Hair  37 0.074596774   0.096858639
##                Orange   2 0.004032258   0.005235602
##        Orange / White   1 0.002016129   0.002617801
##                  Pink   1 0.002016129   0.002617801
##                Purple   1 0.002016129   0.002617801
##                   Red  40 0.080645161   0.104712042
##           Red / White   1 0.002016129   0.002617801
##                Silver   3 0.006048387   0.007853403
##      Strawberry Blond   4 0.008064516   0.010471204
##                 White  10 0.020161290   0.026178010
##                Yellow   2 0.004032258   0.005235602
##                  <NA> 114 0.229838710            NA
```

```r
tabyl(bad_guys$hair_color)
```

```
##  bad_guys$hair_color  n     percent valid_percent
##               Auburn  3 0.014492754   0.019480519
##                Black 42 0.202898551   0.272727273
##         Black / Blue  1 0.004830918   0.006493506
##                blond  1 0.004830918   0.006493506
##                Blond 11 0.053140097   0.071428571
##                 Blue  1 0.004830918   0.006493506
##                Brown 27 0.130434783   0.175324675
##               Brownn  1 0.004830918   0.006493506
##                 Gold  1 0.004830918   0.006493506
##                Green  1 0.004830918   0.006493506
##                 Grey  3 0.014492754   0.019480519
##              No Hair 35 0.169082126   0.227272727
##               Purple  3 0.014492754   0.019480519
##                  Red  9 0.043478261   0.058441558
##           Red / Grey  1 0.004830918   0.006493506
##         Red / Orange  1 0.004830918   0.006493506
##     Strawberry Blond  3 0.014492754   0.019480519
##                White 10 0.048309179   0.064935065
##                 <NA> 53 0.256038647            NA
```
####There are more good guys that are bald.

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight greater than or equal to 450?

```r
big_superhero <- superhero_info %>%
  filter(height > 200 | weight >= 450) %>%
  select(name, height, weight)
big_superhero
```

```
## # A tibble: 60 x 3
##    name          height weight
##    <chr>          <dbl>  <dbl>
##  1 A-Bomb           203    441
##  2 Abomination      203    441
##  3 Alien            244    169
##  4 Amazo            257    173
##  5 Ant-Man          211    122
##  6 Anti-Venom       229    358
##  7 Apocalypse       213    135
##  8 Bane             203    180
##  9 Beta Ray Bill    201    216
## 10 Bloodaxe         218    495
## # ... with 50 more rows
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
filter(superhero_info, height > 300)
```

```
## # A tibble: 8 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Fin F~ Male   red       Kaka~ No Hair      975  Marvel C~ green      good     
## 2 Galac~ Male   black     Cosm~ Black        876  Marvel C~ <NA>       neutral  
## 3 Groot  Male   yellow    Flor~ <NA>         701  Marvel C~ <NA>       good     
## 4 MODOK  Male   white     Cybo~ Brownn       366  Marvel C~ <NA>       bad      
## 5 Onsla~ Male   red       Muta~ No Hair      305  Marvel C~ <NA>       bad      
## 6 Sasqu~ Male   red       <NA>  Orange       305  Marvel C~ <NA>       good     
## 7 Wolfs~ Female green     <NA>  Auburn       366  Marvel C~ <NA>       good     
## 8 Ymir   Male   white     Fros~ No Hair      305. Marvel C~ white      good     
## # ... with 1 more variable: weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

There are not 16 rows in question 10 as the OR operator includes all superheros that meet either of the criteria listed in filter() and not just the superheros that meet both criteria.


```r
filter(superhero_info, weight > 450)
```

```
## # A tibble: 8 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Blood~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
## 2 Darks~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
## 3 Gigan~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
## 4 Hulk   Male   green     Huma~ Green       244   Marvel C~ green      good     
## 5 Jugge~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
## 6 Red H~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
## 7 Sasqu~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
## 8 Wolfs~ Female green     <NA>  Auburn      366   Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```


```r
filter(superhero_info, height > 300 & weight > 450)
```

```
## # A tibble: 2 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Sasqu~ Male   red       <NA>  Orange        305 Marvel C~ <NA>       good     
## 2 Wolfs~ Female green     <NA>  Auburn        366 Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```


## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
h_to_w <- superhero_info %>%
  mutate(height_to_weight = height/weight) %>%
  select(name, height_to_weight) %>%
  arrange(desc(height_to_weight))
head(h_to_w)
```

```
## # A tibble: 6 x 2
##   name           height_to_weight
##   <chr>                     <dbl>
## 1 Groot                    175.  
## 2 Galactus                  54.8 
## 3 Fin Fang Foom             54.2 
## 4 Longshot                   5.22
## 5 Jack-Jack                  5.07
## 6 Rocket Raccoon             4.88
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abin ~
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, F~
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, FA~
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE, ~
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, T~
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, F~
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, FA~
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TRUE~
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, FAL~
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, FA~
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE, F~
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, ~
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE, T~
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, ~
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, ~
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, ~
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
count(superhero_powers %>%
  filter(accelerated_healing == TRUE, durability == TRUE, super_strength == TRUE))
```

```
## # A tibble: 1 x 1
##       n
##   <int>
## 1    97
```

97 superheros have a combination of accelerated healing, durability, and super strength.

## Your Favorite
15. Pick your favorite superhero and let's see their powers!

```r
favorite_superhero <- superhero_powers %>%
  filter(hero_names == 'Spider-Man')
favorite_superhero
```

```
## # A tibble: 1 x 168
##   hero_names agility accelerated_healing lantern_power_ring dimensional_awarene~
##   <chr>      <lgl>   <lgl>               <lgl>              <lgl>               
## 1 Spider-Man TRUE    TRUE                FALSE              FALSE               
## # ... with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>, ...
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
