<!-- README.md is generated from README.Rmd. Please edit that file -->

------------------------------------------------------------------------

gganatogram
-----------

Create anatogram images for different organisms. <br/> For now only human male is available. <br/> This package uses the tissue coordinates from the figure in ArrayExpress Expression Atlas. <br/> <https://www.ebi.ac.uk/gxa/home> <br/> <https://github.com/ebi-gene-expression-group/anatomogram> <br/>

More plot examples can be found at <https://jespermaag.github.io/blog/2018/gganatogram/>

Install
-------

Install from github using devtools.

``` r
## install from Github
devtools::install_github("jespermaag/gganatogram")
```

Usage
-----

This package requires `ggplot2` and `ggpolypath`

``` r
library(ggplot2)
library(ggpolypath)
library(gganatogram)
library(dplyr)
```

In order to use the function gganatogram, you need to have a data frame with organ, colour, and value if you want to.

``` r
organPlot <- data.frame(organ = c("heart", "leukocyte", "nerve", "brain", "liver", "stomach", "colon"), 
 type = c("circulation", "circulation",  "nervous system", "nervous system", "digestion", "digestion", "digestion"), 
 colour = c("red", "red", "purple", "purple", "orange", "orange", "orange"), 
 value = c(10, 5, 1, 8, 2, 5, 5), 
 stringsAsFactors=F)

 head(organPlot)
#>       organ           type colour value
#> 1     heart    circulation    red    10
#> 2 leukocyte    circulation    red     5
#> 3     nerve nervous system purple     1
#> 4     brain nervous system purple     8
#> 5     liver      digestion orange     2
#> 6   stomach      digestion orange     5
```

Using the function gganatogram with the filling the organs based on colour.

``` r
gganatogram(data=organPlot, fillOutline='#a6bddb', organism='human', sex='male', fill="colour")
```

![](figure/organPlot-1.svg)

Of course, we can use the ggplot themes and functions to adjust the plots

``` r
gganatogram(data=organPlot, fillOutline='#a6bddb', organism='human', sex='male', fill="colour") + 
theme_void()
```

![](figure/organPlotvoid-1.svg)

We can also plot all tissues available using hgMale\_key

``` r
hgMale_key$organ
#>  [1] "bone marrow"               "frontal cortex"           
#>  [3] "prefrontal cortex"         "gastroesophageal junction"
#>  [5] "caecum"                    "ileum"                    
#>  [7] "rectum"                    "nose"                     
#>  [9] "tongue"                    "penis"                    
#> [11] "nasal pharynx"             "spinal cord"              
#> [13] "throat"                    "diaphragm"                
#> [15] "liver"                     "stomach"                  
#> [17] "spleen"                    "duodenum"                 
#> [19] "gall bladder"              "pancreas"                 
#> [21] "colon"                     "small intestine"          
#> [23] "appendix"                  "urinary bladder"          
#> [25] "bone"                      "cartilage"                
#> [27] "esophagus"                 "skin"                     
#> [29] "brain"                     "heart"                    
#> [31] "lymph_node"                "skeletal_muscle"          
#> [33] "leukocyte"                 "temporal_lobe"            
#> [35] "atrial_appendage"          "coronary_artery"          
#> [37] "hippocampus"               "vas_deferens"             
#> [39] "seminal_vesicle"           "epididymis"               
#> [41] "tonsil"                    "lung"                     
#> [43] "trachea"                   "bronchus"                 
#> [45] "nerve"                     "kidney"
gganatogram(data=hgMale_key, fillOutline='#a6bddb', organism='human', sex='male', fill="colour") +theme_void()
```

![](figure/organPlotAll-1.svg)

We can also skip plotting the outline of the graph

``` r
organPlot %>%
    dplyr::filter(type %in% c('circulation', 'nervous system')) %>%
gganatogram(outline=F, fillOutline='#a6bddb', organism='human', sex='male', fill="colour") + 
theme_void()
```

![](figure/organPlotSubset-1.svg)

We can fill the tissues based on the values given to each organ

``` r
gganatogram(data=organPlot, fillOutline='#a6bddb', organism='human', sex='male', fill="value") + 
theme_void() +
scale_fill_gradient(low = "white", high = "red")
```

![](figure/organPlotValue-1.svg)

We can also use facet\_wrap to compare groups.
First create add two data frames together with different values and the conditions in the type column

``` r
compareGroups <- rbind(data.frame(organ = c("heart", "leukocyte", "nerve", "brain", "liver", "stomach", "colon"), 
  colour = c("red", "red", "purple", "purple", "orange", "orange", "orange"), 
 value = c(10, 5, 1, 8, 2, 5, 5), 
 type = rep('Normal', 7), 
 stringsAsFactors=F),
 data.frame(organ = c("heart", "leukocyte", "nerve", "brain", "liver", "stomach", "colon"), 
  colour = c("red", "red", "purple", "purple", "orange", "orange", "orange"), 
 value = c(5, 5, 10, 8, 2, 5, 5), 
 type = rep('Cancer', 7), 
 stringsAsFactors=F))
```

``` r
gganatogram(data=compareGroups, fillOutline='#a6bddb', organism='human', sex='male', fill="value") + 
theme_void() +
facet_wrap(~type) +
scale_fill_gradient(low = "white", high = "red") 
```

![](figure/Condition-1.svg)
