HW7
================
Jo, Munsun
2020 11 11

  - nycflights13 패키지에 있는 flights 자료를 이용

<!-- end list -->

``` r
library(nycflights13)
```

    ## Warning: package 'nycflights13' was built under R version 4.0.3

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
flights
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # ... with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

### 1\. dep\_time과 sched\_dep\_time은 특수한 형태로 저장된 자료이다. (519는 5시 19분을 나타냄). 이 두 변수의 시간을 자정을 기준으로 몇 분 후인지를 나타내는 dep\_time\_min과 sched\_dep\_time\_min을 만드시오.

``` r
flights %>% 
  mutate(hr = dep_time %/% 100, # 출발 시간
         min = dep_time %% 100, # 출발 분
         hr_s = sched_dep_time %/% 100, # 예정된 출발 시간
         min_s = sched_dep_time %% 100) %>% # 예정된 출발 분
  transmute(dep_time_min = (hr*60 + min) %% 1440,
            sched_dep_time_min = (hr_s*60 + min_s) %% 1440)
```

    ## # A tibble: 336,776 x 2
    ##    dep_time_min sched_dep_time_min
    ##           <dbl>              <dbl>
    ##  1          317                315
    ##  2          333                329
    ##  3          342                340
    ##  4          344                345
    ##  5          354                360
    ##  6          354                358
    ##  7          355                360
    ##  8          357                360
    ##  9          357                360
    ## 10          358                360
    ## # ... with 336,766 more rows

dep\_time\_min과 sched\_dep\_time\_min은 위와 같습니다.

### 2\. flights 자료에서 일별로 dep\_time이 늦은 비행 5건씩을 뽑고 이를 year, month, day, dep\_time 변수만을 dep\_time이 늦은 순으로 나타내시요.

``` r
flights %>%
  filter(dep_time<2400) %>% 
  group_by(day) %>%
  filter(rank(desc(dep_time))<=5) %>% 
  select(year, month, day, dep_time) %>%
  arrange(desc(dep_time))
```

    ## # A tibble: 146 x 4
    ## # Groups:   day [31]
    ##     year month   day dep_time
    ##    <int> <int> <int>    <int>
    ##  1  2013     1     7     2359
    ##  2  2013     1    12     2359
    ##  3  2013     1    13     2359
    ##  4  2013     1    18     2359
    ##  5  2013     1    19     2359
    ##  6  2013     1    25     2359
    ##  7  2013    10    11     2359
    ##  8  2013    11     5     2359
    ##  9  2013    11    14     2359
    ## 10  2013    11    16     2359
    ## # ... with 136 more rows

일(day)별로 dep\_time이 늦은 비행 5건씩을 뽑고, year, month, day, dep\_time 변수만을
dep\_time이 늦은 순으로 나타내면 위와 같습니다.

``` r
flights %>%
  filter(dep_time < 2400) %>% 
  group_by(day) %>%
  filter(rank(desc(dep_time))<=5) %>% 
  arrange(day)
```

    ## # A tibble: 146 x 19
    ## # Groups:   day [31]
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     3     1     2357           2355         2      451            440
    ##  2  2013     4     1     2358           2359        -1      350            343
    ##  3  2013     6     1     2359           2359         0      332            341
    ##  4  2013     7     1     2359           2049       190      239           2348
    ##  5  2013     8     1     2358           2245        73      112              1
    ##  6  2013    11     2     2358           2359        -1      350            340
    ##  7  2013    12     2     2358           2359        -1      447            437
    ##  8  2013    12     2     2358           2110       168      129           2306
    ##  9  2013     2     2     2359           2359         0      507            437
    ## 10  2013     9     2     2358           2359        -1      354            350
    ## # ... with 136 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

일(day)별로 dep\_time이 늦은 비행 5건씩을 뽑고, day에 대해 오름차순으로 정리하면 위와 같습니다. day(1일,
2일, …)별로 dep\_time이 늦은 5건씩 잘 뽑아낸 것을 확인할 수 있습니다.

### 3\. 각 월별로 도착공항별 도착 delay 건수를 구하시오.

``` r
data3 <- flights %>% 
  group_by(month, dest) %>% 
  filter(arr_delay>0) %>% 
  count(month)
data3
```

    ## # A tibble: 1,101 x 3
    ## # Groups:   month, dest [1,101]
    ##    month dest      n
    ##    <int> <chr> <int>
    ##  1     1 ALB      34
    ##  2     1 ATL     589
    ##  3     1 AUS      80
    ##  4     1 AVL       1
    ##  5     1 BDL      11
    ##  6     1 BHM       8
    ##  7     1 BNA     195
    ##  8     1 BOS     335
    ##  9     1 BQN      34
    ## 10     1 BTV      86
    ## # ... with 1,091 more rows

각 월(month)별로 도착공항(dest)별 도착 delay 건수(n\_arr\_delay)는 위와 같습니다.

### 4\. 3의 데이터에서 각 월별로 도착 delay가 많은 순으로 top 5 도착공항을 뽑으시오.

``` r
data4 <- data3 %>%
  ungroup(dest) %>%  
  filter(rank(desc(n))<=5)
data4
```

    ## # A tibble: 59 x 3
    ## # Groups:   month [12]
    ##    month dest      n
    ##    <int> <chr> <int>
    ##  1     1 ATL     589
    ##  2     1 CLT     484
    ##  3     1 FLL     455
    ##  4     1 MCO     422
    ##  5     1 ORD     547
    ##  6     2 ATL     575
    ##  7     2 CLT     498
    ##  8     2 FLL     528
    ##  9     2 MCO     504
    ## 10     2 ORD     422
    ## # ... with 49 more rows

각 월(month)별로 도착 delay(n\_arr\_delay)가 많은 순으로 뽑은 top 5 도착공항은 위와 같습니다. 단
7월에는 도착 delay가 MCO 도착항공과 ORD 도착항공 둘 다 582회로 동일하기 때문에, 7월은 도착항공이 이 둘을
제외한 총 4개로 나타납니다.

### 5\. 자료가 잘못 입력되었다고 의심될 만큼 빠르거나 느린 비행이 있는지 살펴보려고 한다. 이를 위해 필요한 변수를 만들고 살펴보시오.

  - air\_time: 실제 비행 시간
  - (sched\_arr\_time - sched\_dep\_time): 예정된 비행 시간
  - 새로운 변수 air\_diff = (sched\_arr\_time - sched\_dep\_time) /air\_time)

앞 \#1에서 보았듯이 sched\_arr\_time, sched\_dep\_time은 특수한 형태로 저장된 자료이므로 (519는
5시 19분을 나타냄) 분 단위로 변환한 뒤 계산해줍니다. air\_diff 값이 너무 작거나 크면 이상치로 의심할 수 있습니다.

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v stringr 1.4.0
    ## v tidyr   1.1.2     v forcats 0.5.0
    ## v readr   1.3.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
air_outlier <- flights %>% 
  separate(sched_arr_time, into = c("sched_arr_hour", "sched_arr_minute"), sep = -2) %>% 
  separate(sched_dep_time, into = c("sched_dep_hour", "sched_dep_minute"), sep = -2) %>% 
  mutate(sched_arr_min = as.numeric(sched_arr_hour)*60 + as.numeric(sched_arr_minute),        
         sched_dep_min = as.numeric(sched_dep_hour)*60 + as.numeric(sched_dep_minute),
         air_diff = (sched_arr_min - sched_dep_min )/air_time) %>% 
  select(year, month, day, dep_time, origin, dest, flight, air_diff)

air_outlier
```

    ## # A tibble: 336,776 x 8
    ##     year month   day dep_time origin dest  flight air_diff
    ##    <int> <int> <int>    <int> <chr>  <chr>  <int>    <dbl>
    ##  1  2013     1     1      517 EWR    IAH     1545    0.811
    ##  2  2013     1     1      533 LGA    IAH     1714    0.797
    ##  3  2013     1     1      542 JFK    MIA     1141    1.19 
    ##  4  2013     1     1      544 JFK    BQN      725    1.51 
    ##  5  2013     1     1      554 LGA    ATL      461    1.35 
    ##  6  2013     1     1      554 EWR    ORD     1696    0.6  
    ##  7  2013     1     1      555 EWR    FLL      507    1.10 
    ##  8  2013     1     1      557 LGA    IAD     5708    1.57 
    ##  9  2013     1     1      557 JFK    MCO       79    1.19 
    ## 10  2013     1     1      558 LGA    ORD      301    0.761
    ## # ... with 336,766 more rows

``` r
air_outlier %>% select(air_diff) %>% summary()
```

    ##     air_diff      
    ##  Min.   :-12.528  
    ##  1st Qu.:  0.790  
    ##  Median :  1.208  
    ##  Mean   :  1.126  
    ##  3rd Qu.:  1.442  
    ##  Max.   :  4.227  
    ##  NA's   :13555

``` r
air_outlier %>% ggplot(aes(air_diff)) + geom_histogram(binwidth = 0.01, na.rm = TRUE)
```

![](EX7-nycflights13_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->
