---
title: "Lab 4 Homework"
author: "Evan Roybal"
date: "2022-01-18"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**


```r
homerange <- read.csv('data/Tamburelloetal_HomeRangeDatabase.csv')
```


**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

####Dimensions

```r
dim(homerange)
```

```
## [1] 569  24
```

####Column Names

```r
colnames(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```

####Variable Classes

```r
lapply(homerange, class)
```

```
## $taxon
## [1] "character"
## 
## $common.name
## [1] "character"
## 
## $class
## [1] "character"
## 
## $order
## [1] "character"
## 
## $family
## [1] "character"
## 
## $genus
## [1] "character"
## 
## $species
## [1] "character"
## 
## $primarymethod
## [1] "character"
## 
## $N
## [1] "character"
## 
## $mean.mass.g
## [1] "numeric"
## 
## $log10.mass
## [1] "numeric"
## 
## $alternative.mass.reference
## [1] "character"
## 
## $mean.hra.m2
## [1] "numeric"
## 
## $log10.hra
## [1] "numeric"
## 
## $hra.reference
## [1] "character"
## 
## $realm
## [1] "character"
## 
## $thermoregulation
## [1] "character"
## 
## $locomotion
## [1] "character"
## 
## $trophic.guild
## [1] "character"
## 
## $dimension
## [1] "character"
## 
## $preymass
## [1] "numeric"
## 
## $log10.preymass
## [1] "numeric"
## 
## $PPMR
## [1] "numeric"
## 
## $prey.size.reference
## [1] "character"
```

####Statistical Summary

```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```


**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

####Changing classes of taxon and order to factor

```r
homerange$taxon <- as.factor(homerange$taxon)
homerange$order <- as.factor(homerange$order)
```

####Displaying levels of taxon and order variables

```r
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```

```r
levels(homerange$order)
```

```
##  [1] "accipitriformes"    "afrosoricida"       "anguilliformes"    
##  [4] "anseriformes"       "apterygiformes"     "artiodactyla"      
##  [7] "caprimulgiformes"   "carnivora"          "charadriiformes"   
## [10] "columbidormes"      "columbiformes"      "coraciiformes"     
## [13] "cuculiformes"       "cypriniformes"      "dasyuromorpha"     
## [16] "dasyuromorpia"      "didelphimorphia"    "diprodontia"       
## [19] "diprotodontia"      "erinaceomorpha"     "esociformes"       
## [22] "falconiformes"      "gadiformes"         "galliformes"       
## [25] "gruiformes"         "lagomorpha"         "macroscelidea"     
## [28] "monotrematae"       "passeriformes"      "pelecaniformes"    
## [31] "peramelemorphia"    "perciformes"        "perissodactyla"    
## [34] "piciformes"         "pilosa"             "proboscidea"       
## [37] "psittaciformes"     "rheiformes"         "roden"             
## [40] "rodentia"           "salmoniformes"      "scorpaeniformes"   
## [43] "siluriformes"       "soricomorpha"       "squamata"          
## [46] "strigiformes"       "struthioniformes"   "syngnathiformes"   
## [49] "testudines"         "tetraodontiformes " "tinamiformes"
```


**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

####Taxa represented in homerange df

```r
unique(homerange$taxon)
```

```
## [1] lake fishes   river fishes  marine fishes birds         mammals      
## [6] lizards       snakes        turtles       tortoises    
## 9 Levels: birds lake fishes lizards mammals marine fishes ... turtles
```
####creation of taxa df

```r
taxa <- homerange %>%
  select(taxon, common.name, class, order, family, genus, species)
head(taxa)
```

```
##          taxon         common.name          class          order       family
## 1  lake fishes        american eel actinopterygii anguilliformes  anguillidae
## 2 river fishes  blacktail redhorse actinopterygii  cypriniformes catostomidae
## 3 river fishes central stoneroller actinopterygii  cypriniformes   cyprinidae
## 4 river fishes       rosyside dace actinopterygii  cypriniformes   cyprinidae
## 5 river fishes       longnose dace actinopterygii  cypriniformes   cyprinidae
## 6 river fishes        muskellunge  actinopterygii    esociformes     esocidae
##         genus     species
## 1    anguilla    rostrata
## 2   moxostoma   poecilura
## 3  campostoma    anomalum
## 4 clinostomus funduloides
## 5 rhinichthys  cataractae
## 6        esox masquinongy
```


**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

#table of taxon

```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```


**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

####table of trophic guild observations

```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```
342 species in the carnivore trophic guild and 227 species in the herbivore trophic guild.


**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

#Creation of carnivore df

```r
carnivores <- homerange %>%
  filter(trophic.guild == 'carnivore')
head(carnivores)
```

```
##          taxon         common.name          class          order       family
## 1  lake fishes        american eel actinopterygii anguilliformes  anguillidae
## 2 river fishes  blacktail redhorse actinopterygii  cypriniformes catostomidae
## 3 river fishes central stoneroller actinopterygii  cypriniformes   cyprinidae
## 4 river fishes       rosyside dace actinopterygii  cypriniformes   cyprinidae
## 5 river fishes       longnose dace actinopterygii  cypriniformes   cyprinidae
## 6 river fishes        muskellunge  actinopterygii    esociformes     esocidae
##         genus     species  primarymethod    N mean.mass.g log10.mass
## 1    anguilla    rostrata      telemetry   16         887   2.947924
## 2   moxostoma   poecilura mark-recapture <NA>         562   2.749736
## 3  campostoma    anomalum mark-recapture   20          34   1.531479
## 4 clinostomus funduloides mark-recapture   26           4   0.602060
## 5 rhinichthys  cataractae mark-recapture   17           4   0.602060
## 6        esox masquinongy      telemetry    5        3525   3.547159
##   alternative.mass.reference mean.hra.m2 log10.hra
## 1                       <NA>   282750.00  5.451403
## 2                       <NA>      282.10  2.450403
## 3                       <NA>      116.11  2.064870
## 4                       <NA>      125.50  2.098644
## 5                       <NA>       87.10  1.940018
## 6                       <NA>    39343.50  4.594873
##                                                                                                                                hra.reference
## 1 Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52:1499-1508.
## 2 Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52:1499-1508.
## 3 Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52:1499-1508.
## 4 Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52:1499-1508.
## 5 Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52:1499-1508.
## 6 Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52:1499-1508.
##     realm thermoregulation locomotion trophic.guild dimension preymass
## 1 aquatic        ectotherm   swimming     carnivore        3D       NA
## 2 aquatic        ectotherm   swimming     carnivore        2D       NA
## 3 aquatic        ectotherm   swimming     carnivore        2D       NA
## 4 aquatic        ectotherm   swimming     carnivore        2D       NA
## 5 aquatic        ectotherm   swimming     carnivore        2D       NA
## 6 aquatic        ectotherm   swimming     carnivore        2D       NA
##   log10.preymass PPMR prey.size.reference
## 1             NA   NA                <NA>
## 2             NA   NA                <NA>
## 3             NA   NA                <NA>
## 4             NA   NA                <NA>
## 5             NA   NA                <NA>
## 6             NA   NA                <NA>
```

#creation of herbivore df

```r
herbivores <- homerange %>%
  filter(trophic.guild == 'herbivore')
head(herbivores)
```

```
##           taxon             common.name          class       order
## 1 marine fishes       lined surgeonfish actinopterygii perciformes
## 2 marine fishes orangespine unicornfish actinopterygii perciformes
## 3 marine fishes   bluespine unicornfish actinopterygii perciformes
## 4 marine fishes           redlip blenny actinopterygii perciformes
## 5 marine fishes            bermuda chub actinopterygii perciformes
## 6 marine fishes              cherubfish actinopterygii perciformes
##          family         genus    species      primarymethod    N mean.mass.g
## 1  acanthuridae    acanthurus   lineatus direct observation <NA>      109.04
## 2  acanthuridae          naso  lituratus          telemetry    8      772.16
## 3  acanthuridae          naso  unicornis          telemetry    7      151.84
## 4     blennidae ophioblennius atlanticus direct observation   20        6.20
## 5    kyphosidae      kyphosus  sectatrix          telemetry   11     1086.71
## 6 pomacanthidae    centropyge       argi direct observation <NA>        2.50
##   log10.mass alternative.mass.reference mean.hra.m2   log10.hra
## 1  2.0375858                       <NA>       11.13  1.04649516
## 2  2.8877073                       <NA>    32092.86  4.50640842
## 3  2.1813862                       <NA>    17900.00  4.25285303
## 4  0.7923917                       <NA>        0.52 -0.28399666
## 5  3.0361137                       <NA>    34423.00  4.53684872
## 6  0.3979400                       <NA>        1.13  0.05307844
##                                                                                                                                                                                                                              hra.reference
## 1                                                                                             Nursall JR. 1974. Some Territorial Behavioral Attributes of the Surgeonfish Acanthurus lineatus at Heron Island, Queensland. Copeia 950-959.
## 2                                    Marshell A, Mills JS, Rhodes KL, et al. 2011. Passive acoustic telemetry reveals highly variable home range and movement patterns among unicornfish within a marine reserve. Coral Reefs 30, 631-642.
## 3                                    Marshell A, Mills JS, Rhodes KL, et al. 2011. Passive acoustic telemetry reveals highly variable home range and movement patterns among unicornfish within a marine reserve. Coral Reefs 30, 631-642.
## 4                                                                                           Nursall JR. 1977. Territoriality in Redlip blennies (Ophioblennius atlanticus - Pisces : Blenniidae). Journal of Zoology, London 182, 205-223.
## 5 Eristhee N, Oxenford HA. 2001. Home range size and use of space by Bermuda chub Kyphosus sectatrix (L.) in two marine reserves in the Soufrière Marine Management Area, St Lucia, West Indies. Journal of Fisheries Biology 59, 129-151.
## 6                                                                                                                   Luckhurst BE, Luckhurst K. 1978. Diurnal Space Utilization in Coral Reef Fish Communities. Marine Biology 49, 325-332.
##     realm thermoregulation locomotion trophic.guild dimension preymass
## 1 aquatic        ectotherm   swimming     herbivore        2D       NA
## 2 aquatic        ectotherm   swimming     herbivore        2D       NA
## 3 aquatic        ectotherm   swimming     herbivore        2D       NA
## 4 aquatic        ectotherm   swimming     herbivore        2D       NA
## 5 aquatic        ectotherm   swimming     herbivore        2D       NA
## 6 aquatic        ectotherm   swimming     herbivore        2D       NA
##   log10.preymass PPMR prey.size.reference
## 1             NA   NA                <NA>
## 2             NA   NA                <NA>
## 3             NA   NA                <NA>
## 4             NA   NA                <NA>
## 5             NA   NA                <NA>
## 6             NA   NA                <NA>
```



**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

####comparison of mean.hra.m2

```r
mean(carnivores$mean.hra.m2, na.rm = TRUE)
```

```
## [1] 13039918
```

```r
mean(herbivores$mean.hra.m2, na.rm = TRUE)
```

```
## [1] 34137012
```
Herbivores, on average, have a larger mean.hra.m2


**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

####creation of deer df

```r
deer <- homerange %>%
  select(mean.mass.g, log10.mass, family, genus, species) %>%
  filter(family == 'cervidae') %>%
  arrange(desc(log10.mass))
head(deer)
```

```
##   mean.mass.g log10.mass   family      genus     species
## 1   307227.44    5.48746 cervidae      alces       alces
## 2   234757.78    5.37062 cervidae     cervus     elaphus
## 3   102058.69    5.00885 cervidae   rangifer    tarandus
## 4    87884.04    4.94391 cervidae odocoileus virginianus
## 5    71449.63    4.85400 cervidae       dama        dama
## 6    62823.19    4.79812 cervidae       axis        axis
```

####Finding largest deer

```r
slice_max(deer, mean.mass.g)
```

```
##   mean.mass.g log10.mass   family genus species
## 1    307227.4    5.48746 cervidae alces   alces
```

```r
homerange %>%
  select(common.name, species) %>%
  filter(species == 'alces')
```

```
##   common.name species
## 1       moose   alces
```
The largest deer is the alces alces with the common name of moose.


**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    

####creation of df with only snakes and homerange

```r
snakes <- homerange %>%
  select(taxon, common.name, genus, species, mean.hra.m2) %>%
  filter(taxon == 'snakes')
head(snakes)
```

```
##    taxon          common.name      genus                  species mean.hra.m2
## 1 snakes   western worm snake  carphopis                   vermis         700
## 2 snakes   eastern worm snake  carphopis                  viridis         253
## 3 snakes                racer    coluber              constrictor      151000
## 4 snakes yellow bellied racer    coluber constrictor flaviventris      114500
## 5 snakes       ringneck snake  diadophis                punctatus        6476
## 6 snakes eastern indigo snake drymarchon                  couperi     1853000
```

####Finding snake species with smallest homerange

```r
slice_min(snakes,mean.hra.m2)
```

```
##    taxon         common.name genus    species mean.hra.m2
## 1 snakes namaqua dwarf adder bitis schneideri         200
```

The bitis schneideri or namaqua dwarf adder has the smallest homerange. It is a venemous viper that lives in south part of Africa and is possibly the smallest viper in the world. The species has an above average mortality rate and its venom is possibly non-lethal for humans.
source: https://en.wikipedia.org/wiki/Bitis_schneideri


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
