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

![](index_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-1.png)

Max scores of Universities in Istanbul
--------------------------------------

``` r
question2 <- osym_data_2017 %>%
  select(university_name, max_score, city) %>%
  filter(city=='İSTANBUL') %>%
  group_by(university_name) %>%
  summarise(max_puan=max(max_score)) %>%
  arrange(desc(max_puan))

question2
```

    ## # A tibble: 51 x 2
    ##                              university_name max_puan
    ##                                        <chr>    <dbl>
    ##  1                          KOÇ ÜNÝVERSÝTESÝ 569.1112
    ##  2                     ÝSTANBUL ÜNÝVERSÝTESÝ 564.0145
    ##  3                     BOÐAZÝÇÝ ÜNÝVERSÝTESÝ 562.5765
    ##  4             ÝSTANBUL MEDÝPOL ÜNÝVERSÝTESÝ 559.4780
    ##  5                  GALATASARAY ÜNÝVERSÝTESÝ 556.0948
    ##  6 ACIBADEM MEHMET ALÝ AYDINLAR ÜNÝVERSÝTESÝ 542.3482
    ##  7                      SABANCI ÜNÝVERSÝTESÝ 538.7725
    ##  8                     YEDÝTEPE ÜNÝVERSÝTESÝ 531.3691
    ##  9                   BAHÇEÞEHÝR ÜNÝVERSÝTESÝ 530.4845
    ## 10               ÝSTANBUL AYDIN ÜNÝVERSÝTESÝ 525.5809
    ## # ... with 41 more rows

Let's visualize this data in barchart.

``` r
ggplot(question2, aes(x=reorder(university_name,-max_puan), y=max_puan)) +
  geom_bar(stat = "identity", aes(fill=ifelse(question2$university_name == 'MEF ÜNİVERSİTESİ',"blue","red"))) +
  labs(title="Maximum score of each university",x="University",y="Maximum score",fill="") +
  theme (axis.text.x=element_text (angle=-90,vjust=2,hjust=0))
```

![](index_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-5-1.png)

Number of departments of Universities in Istanbul
-------------------------------------------------

``` r
question3 <- osym_data_2017 %>% 
  select(university_name,general_quota,city) %>% 
  filter(city=='İSTANBUL') %>%
  group_by(university_name) %>% 
  summarise(bolum_sayisi=n(),general_quota=sum(general_quota)) %>% 
  arrange(desc(general_quota))

question3
```

    ## # A tibble: 51 x 3
    ##                  university_name bolum_sayisi general_quota
    ##                            <chr>        <int>         <int>
    ##  1         ÝSTANBUL ÜNÝVERSÝTESÝ          138         17809
    ##  2          MARMARA ÜNÝVERSÝTESÝ           80          6200
    ##  3 ÝSTANBUL MEDÝPOL ÜNÝVERSÝTESÝ          155          4495
    ##  4 ÝSTANBUL GELÝÞÝM ÜNÝVERSÝTESÝ          212          3950
    ##  5          BEYKENT ÜNÝVERSÝTESÝ          169          3811
    ##  6  ÝSTANBUL TEKNÝK ÜNÝVERSÝTESÝ           76          3684
    ##  7    YILDIZ TEKNÝK ÜNÝVERSÝTESÝ           53          3652
    ##  8   ÝSTANBUL AYDIN ÜNÝVERSÝTESÝ          154          3578
    ##  9         YEDÝTEPE ÜNÝVERSÝTESÝ          165          3442
    ## 10       BAHÇEÞEHÝR ÜNÝVERSÝTESÝ          122          2774
    ## # ... with 41 more rows

Let's visualize this data in barchart.

``` r
ggplot(question3,aes(x=reorder(university_name,-bolum_sayisi),y=bolum_sayisi))+
  geom_bar(stat ="identity",aes(fill=ifelse(question3$university_name=='MEF ÜNİVERSİTESİ',"blue","red")))+
  labs(title='Number of Departments of Universities in İstanbul',x='University',y='Departments',fill="")+
  theme(axis.text.x=element_text(angle=-90,vjust=0.5,hjust=0))
```

![](index_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-7-1.png)
