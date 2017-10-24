Case Study: Welcome to University
================
R Junkies

Introduction
------------

In this case study we are going to explore university entrance examinations (YGS/LYS) data from 2017. Data consists of undergraduate programs offered in 2017. Each program offers an availability (i.e. quota). Then students get placed according to their lists and their scores. Each program is filled with the students ranked by their scores until placements are equal to availability. Student placed to a a program with the highest score forms the maximum score of that program and the last student to be placed forms the minimum score.

Data Loading
------------

``` r
# Download from GitHub (do it only once)
download.file("https://mef-bda503.github.io/files/osym_data_2017.RData", "osym_data_2017.RData")
# Install tidyverse if not already installed
if (!("tidyverse" %in% installed.packages())) {
    install.packages("tidyverse", repos = "https://cran.r-project.org")
}
# Load tidyverse package
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 3.4.2

    ## Warning: package 'ggplot2' was built under R version 3.4.2

    ## Warning: package 'tibble' was built under R version 3.4.2

    ## Warning: package 'tidyr' was built under R version 3.4.2

    ## Warning: package 'readr' was built under R version 3.4.2

    ## Warning: package 'purrr' was built under R version 3.4.2

    ## Warning: package 'dplyr' was built under R version 3.4.2

``` r
# Load the data
load("osym_data_2017.RData")
```

Quota of Universities in Istanbul
---------------------------------

This data shows quota of universities in Istanbul

``` r
university_quota <- osym_data_2017 %>%
    group_by(university_name) %>%
    filter(city=='İSTANBUL') %>%
    summarise(count=n()) %>%
    arrange(desc(count))
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.2

``` r
university_quota
```

    ## # A tibble: 51 x 2
    ##                  university_name count
    ##                            <chr> <int>
    ##  1 ÝSTANBUL GELÝÞÝM ÜNÝVERSÝTESÝ   212
    ##  2             OKAN ÜNÝVERSÝTESÝ   172
    ##  3          BEYKENT ÜNÝVERSÝTESÝ   169
    ##  4         YEDÝTEPE ÜNÝVERSÝTESÝ   165
    ##  5 ÝSTANBUL MEDÝPOL ÜNÝVERSÝTESÝ   155
    ##  6   ÝSTANBUL AYDIN ÜNÝVERSÝTESÝ   154
    ##  7         ÝSTANBUL ÜNÝVERSÝTESÝ   138
    ##  8    ÝSTANBUL AREL ÜNÝVERSÝTESÝ   135
    ##  9   ÝSTANBUL BÝLGÝ ÜNÝVERSÝTESÝ   131
    ## 10          MALTEPE ÜNÝVERSÝTESÝ   123
    ## # ... with 41 more rows

Let's visualize this data in barchart.

``` r
ggplot(data=university_quota,aes(x=reorder(university_name,-count) , y=count)) +
  geom_bar(stat='identity') +
             labs(title="Quota of University in Istanbul",x="University Name",y="Count", fill="") +
             theme (axis.text.x=element_text (angle=-90,vjust=0.5,hjust=0))
```

![](CaseStudy_Week03_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-1.png)
