HW6
================
Jo, Munsun
2020 11 4

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
library(ggplot2)
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

### 1\. flights자료에서 출발이 90분 이상 늦어진 비행 자료 중 year, month, day, dep\_delay, arr\_delay, carrier, origin, dest 만을 뽑아서 Late에 저장하시오.

``` r
Late <- flights %>% 
  filter(dep_delay>=90) %>% 
  select(year, month, day, dep_delay, arr_delay, carrier, origin, dest)
Late
```

    ## # A tibble: 16,061 x 8
    ##     year month   day dep_delay arr_delay carrier origin dest 
    ##    <int> <int> <int>     <dbl>     <dbl> <chr>   <chr>  <chr>
    ##  1  2013     1     1       101       137 MQ      LGA    CLT  
    ##  2  2013     1     1       853       851 MQ      JFK    BWI  
    ##  3  2013     1     1       144       123 UA      EWR    BOS  
    ##  4  2013     1     1       134       145 UA      LGA    IAH  
    ##  5  2013     1     1        96        78 EV      EWR    SAV  
    ##  6  2013     1     1       115       127 EV      EWR    RIC  
    ##  7  2013     1     1       105       125 B6      EWR    MCO  
    ##  8  2013     1     1       122       115 B6      JFK    SJU  
    ##  9  2013     1     1       119       123 EV      JFK    IAD  
    ## 10  2013     1     1        91        61 B6      JFK    SJU  
    ## # ... with 16,051 more rows

Late 자료의 dep\_delay 값은 모두 90(분) 이상인 것을 알 수 있습니다.

### 2\. flights 자료에서 시카고 도착의 비행만을 뽑아서 Chicago에 저장하시오.

  - Chicago의 공항코드: ORD, MDW
  - 자료 중 year, month, day, dep\_delay, arr\_delay, carrier, origin,
    dest만을 저장

<!-- end list -->

``` r
Chicago <- flights %>%
  filter(dest %in% c("ORD","MDW")) %>% # filter(dest=="ORD" | dest=="MDW")
  select(year, month, day, dep_delay, arr_delay, carrier, origin, dest)
Chicago
```

    ## # A tibble: 21,396 x 8
    ##     year month   day dep_delay arr_delay carrier origin dest 
    ##    <int> <int> <int>     <dbl>     <dbl> <chr>   <chr>  <chr>
    ##  1  2013     1     1        -4        12 UA      EWR    ORD  
    ##  2  2013     1     1        -2         8 AA      LGA    ORD  
    ##  3  2013     1     1         8        32 MQ      EWR    ORD  
    ##  4  2013     1     1        -1        14 AA      LGA    ORD  
    ##  5  2013     1     1        -4         4 AA      LGA    ORD  
    ##  6  2013     1     1         9        20 UA      LGA    ORD  
    ##  7  2013     1     1         2        21 UA      EWR    ORD  
    ##  8  2013     1     1        -6       -12 AA      LGA    ORD  
    ##  9  2013     1     1        39        49 MQ      EWR    ORD  
    ## 10  2013     1     1        -2        15 B6      JFK    ORD  
    ## # ... with 21,386 more rows

Chicago 자료의 dest 값은 ORD 아니면 MDW임을 알 수 있습니다.

### 3\. flights 자료에서 출발이 90분 이상 늦어진 비행이 가장 많은 항공사를 알아보려고 한다. Late 자료를 이용하여 각 항공사별로 90분 이상 늦어진 비행 건수를 구하고 이를 건수가 많은 항공사부터 나열하시오.

``` r
Late %>%
  group_by(carrier) %>%
  count() %>% # summarise(n = n())
  arrange(desc(n))
```

    ## # A tibble: 16 x 2
    ## # Groups:   carrier [16]
    ##    carrier     n
    ##    <chr>   <int>
    ##  1 EV       4179
    ##  2 B6       2725
    ##  3 UA       2238
    ##  4 DL       1672
    ##  5 9E       1231
    ##  6 AA       1203
    ##  7 MQ       1129
    ##  8 WN        677
    ##  9 US        415
    ## 10 VX        258
    ## 11 FL        214
    ## 12 F9         45
    ## 13 YV         42
    ## 14 AS         23
    ## 15 HA          8
    ## 16 OO          2

각 항공사별로 90분 이상 늦어진 비행 건수를 구하고 이를 건수가 많은 항공사부터 나열하면, EV - B6 - UA - DL -
9E - AA - MQ - WN - US - VX 순임을 알 수 있습니다.

### 4\. 30분 이상 늦게 출발했으나 늦어진 출발시간 중 10분 이상을 비행으로 단축한 비행을 추출하여 비행거리가 큰 순서대로 나열하시오.

  - month, day, distance, dest, dep\_delay, arr\_delay 만 표시

<!-- end list -->

``` r
filter(flights, dep_delay>=30) %>% 
  mutate(gain=arr_delay-dep_delay) %>%
  filter(gain<=(-10)) %>%
  arrange(desc(distance)) %>%
  select(month, day, distance, dest, dep_delay, arr_delay)
```

    ## # A tibble: 20,618 x 6
    ##    month   day distance dest  dep_delay arr_delay
    ##    <int> <int>    <dbl> <chr>     <dbl>     <dbl>
    ##  1     1     6     4983 HNL          79        28
    ##  2     1     7     4983 HNL         102        50
    ##  3     1     9     4983 HNL        1301      1272
    ##  4     1    18     4983 HNL         123        65
    ##  5     1    23     4983 HNL         101        82
    ##  6     2     9     4983 HNL         186       154
    ##  7     2    23     4983 HNL         206       126
    ##  8     2    24     4983 HNL          36       -33
    ##  9     2    26     4983 HNL          60       -27
    ## 10     3     8     4983 HNL          48        35
    ## # ... with 20,608 more rows

30분 이상 늦게 출발했으나( dep\_delay\>=30 ) 늦어진 출발시간 중 10분 이상을 비행으로 단축(
(arr\_delay-dep\_delay)\<=(-10) )한 비행을 추출하여 비행거리가 큰 순서대로 나열하면, 위와 같았습니다.

### 5\. 60분 이상의 출발지연 건수가 가장 많은 달을 찾고 해당 달에 어떤 항공사(carrier)가 60분 이상의 지연이 가장 많은지를 알아보고자 한다. 이를 위한 코드를 작성하고 해당 달과 해당 항공사를 찾으시오.

``` r
filter(flights, dep_delay>=60) %>%
  group_by(month) %>% 
  count() %>% # summarize(n=n()) 
  arrange(desc(n)) 
```

    ## # A tibble: 12 x 2
    ## # Groups:   month [12]
    ##    month     n
    ##    <int> <int>
    ##  1     7  3877
    ##  2     6  3555
    ##  3    12  2597
    ##  4     4  2572
    ##  5     3  2391
    ##  6     5  2357
    ##  7     8  2338
    ##  8     1  1852
    ##  9     2  1688
    ## 10    10  1366
    ## 11     9  1345
    ## 12    11  1121

  - 60분 이상의 출발지연 건수가 가장 많은 달은 7월입니다.

<!-- end list -->

``` r
filter(flights, month==7, dep_delay>=60) %>%
  group_by(carrier) %>% 
  count() %>% # summarize(n=n()) 
  arrange(desc(n)) 
```

    ## # A tibble: 15 x 2
    ## # Groups:   carrier [15]
    ##    carrier     n
    ##    <chr>   <int>
    ##  1 EV        787
    ##  2 B6        783
    ##  3 UA        578
    ##  4 DL        507
    ##  5 MQ        303
    ##  6 9E        263
    ##  7 AA        236
    ##  8 WN        139
    ##  9 US        127
    ## 10 VX         74
    ## 11 FL         56
    ## 12 YV         11
    ## 13 F9          9
    ## 14 AS          3
    ## 15 HA          1

  - 해당 달(7월)에 60분 이상의 출발지연이 가장 많은 항공사(carrier)는 EV입니다.

### 6\. 여름(6,7,8월)에 도착지 별로 도착이 지연되는 정도를 파악하고자 한다. 비행의 도착지 별로 월별 도착지연시간의 평균을 구하고 이를 적절한 그림으로 나타낸 후 그림을 해석하시오.

``` r
summer_delay <- flights %>% 
  filter(month %in% c(6,7,8)) %>%
  group_by(month, dest) %>%
  summarize(month_delay=mean(arr_delay, na.rm=TRUE))
```

    ## `summarise()` regrouping output by 'month' (override with `.groups` argument)

``` r
summer_delay
```

    ## # A tibble: 280 x 3
    ## # Groups:   month [3]
    ##    month dest  month_delay
    ##    <int> <chr>       <dbl>
    ##  1     6 ABQ         2.93 
    ##  2     6 ACK        13.0  
    ##  3     6 ALB         0.242
    ##  4     6 ATL        25.6  
    ##  5     6 AUS        13.1  
    ##  6     6 AVL        12.6  
    ##  7     6 BDL        13.3  
    ##  8     6 BGR         7.73 
    ##  9     6 BHM        43    
    ## 10     6 BNA        26.4  
    ## # ... with 270 more rows

``` r
summer_delay %>%
  ggplot(aes(x=as.factor(month), y=month_delay)) +
  geom_boxplot() +
  xlab("month") + 
  labs(title="비행 도착지별 월별 도착지연시간의 평균 상자그림") +
  theme(plot.title = element_text(face = "bold", hjust = 0.5))
```

    ## Warning: Removed 1 rows containing non-finite values (stat_boxplot).

![](EX6-nycflights13_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

비행 도착지별 월별 도착지연시간의 평균 상자그림에 따르면, 6, 7월에 비해 8월에 도착지연시간이 짧은 편입니다.

``` r
summer_delay %>% 
  ggplot(aes(x= dest, y = month_delay)) +
  geom_bar(aes(fill=as.factor(month)), stat='identity') +
  facet_wrap(~month, ncol=1) +
  labs(title = '비행 도착지별 월별 도착지연시간의 평균 바 차트') +
  theme(axis.text.x = element_text(angle = 60, hjust = 1, size = 4),
        plot.title = element_text(face = "bold", hjust = 0.5), 
        legend.position = 'top', legend.title = element_blank())
```

    ## Warning: Removed 1 rows containing missing values (position_stack).

![](EX6-nycflights13_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

비행 도착지별 월별 도착지연시간의 평균 Bar chart에 의하면 6, 7, 8월에는 도착지 대부분에서 도착지연이 발생하며,
CAE(Columbia Metropolitan Airport)에서 도착지연이 가장 길게 발생합니다.
