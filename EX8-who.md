HW8
================
Jo, Munsun
2020 11 19

### 1\. who 자료 중 2006 \~ 2010의 자료만을 뽑아 NewWho에 저장하시오.

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.2
    ## v tidyr   1.1.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(tidyr)
head(who)
```

    ## # A tibble: 6 x 60
    ##   country iso2  iso3   year new_sp_m014 new_sp_m1524 new_sp_m2534 new_sp_m3544
    ##   <chr>   <chr> <chr> <int>       <int>        <int>        <int>        <int>
    ## 1 Afghan~ AF    AFG    1980          NA           NA           NA           NA
    ## 2 Afghan~ AF    AFG    1981          NA           NA           NA           NA
    ## 3 Afghan~ AF    AFG    1982          NA           NA           NA           NA
    ## 4 Afghan~ AF    AFG    1983          NA           NA           NA           NA
    ## 5 Afghan~ AF    AFG    1984          NA           NA           NA           NA
    ## 6 Afghan~ AF    AFG    1985          NA           NA           NA           NA
    ## # ... with 52 more variables: new_sp_m4554 <int>, new_sp_m5564 <int>,
    ## #   new_sp_m65 <int>, new_sp_f014 <int>, new_sp_f1524 <int>,
    ## #   new_sp_f2534 <int>, new_sp_f3544 <int>, new_sp_f4554 <int>,
    ## #   new_sp_f5564 <int>, new_sp_f65 <int>, new_sn_m014 <int>,
    ## #   new_sn_m1524 <int>, new_sn_m2534 <int>, new_sn_m3544 <int>,
    ## #   new_sn_m4554 <int>, new_sn_m5564 <int>, new_sn_m65 <int>,
    ## #   new_sn_f014 <int>, new_sn_f1524 <int>, new_sn_f2534 <int>,
    ## #   new_sn_f3544 <int>, new_sn_f4554 <int>, new_sn_f5564 <int>,
    ## #   new_sn_f65 <int>, new_ep_m014 <int>, new_ep_m1524 <int>,
    ## #   new_ep_m2534 <int>, new_ep_m3544 <int>, new_ep_m4554 <int>,
    ## #   new_ep_m5564 <int>, new_ep_m65 <int>, new_ep_f014 <int>,
    ## #   new_ep_f1524 <int>, new_ep_f2534 <int>, new_ep_f3544 <int>,
    ## #   new_ep_f4554 <int>, new_ep_f5564 <int>, new_ep_f65 <int>,
    ## #   newrel_m014 <int>, newrel_m1524 <int>, newrel_m2534 <int>,
    ## #   newrel_m3544 <int>, newrel_m4554 <int>, newrel_m5564 <int>,
    ## #   newrel_m65 <int>, newrel_f014 <int>, newrel_f1524 <int>,
    ## #   newrel_f2534 <int>, newrel_f3544 <int>, newrel_f4554 <int>,
    ## #   newrel_f5564 <int>, newrel_f65 <int>

``` r
NewWho <- who %>%
  filter(year %in% c(2006:2010), na.rm = TRUE)
NewWho
```

    ## # A tibble: 1,072 x 60
    ##    country iso2  iso3   year new_sp_m014 new_sp_m1524 new_sp_m2534 new_sp_m3544
    ##    <chr>   <chr> <chr> <int>       <int>        <int>        <int>        <int>
    ##  1 Afghan~ AF    AFG    2006         193          837          791          574
    ##  2 Afghan~ AF    AFG    2007         186          856          840          597
    ##  3 Afghan~ AF    AFG    2008         187          941          773          545
    ##  4 Afghan~ AF    AFG    2009         200          906          705          499
    ##  5 Afghan~ AF    AFG    2010         197          986          819          491
    ##  6 Albania AL    ALB    2006           5           24           19           22
    ##  7 Albania AL    ALB    2007           0           19           13           16
    ##  8 Albania AL    ALB    2008           1           23           26           13
    ##  9 Albania AL    ALB    2009           0           22           21           12
    ## 10 Albania AL    ALB    2010           0           28           17           14
    ## # ... with 1,062 more rows, and 52 more variables: new_sp_m4554 <int>,
    ## #   new_sp_m5564 <int>, new_sp_m65 <int>, new_sp_f014 <int>,
    ## #   new_sp_f1524 <int>, new_sp_f2534 <int>, new_sp_f3544 <int>,
    ## #   new_sp_f4554 <int>, new_sp_f5564 <int>, new_sp_f65 <int>,
    ## #   new_sn_m014 <int>, new_sn_m1524 <int>, new_sn_m2534 <int>,
    ## #   new_sn_m3544 <int>, new_sn_m4554 <int>, new_sn_m5564 <int>,
    ## #   new_sn_m65 <int>, new_sn_f014 <int>, new_sn_f1524 <int>,
    ## #   new_sn_f2534 <int>, new_sn_f3544 <int>, new_sn_f4554 <int>,
    ## #   new_sn_f5564 <int>, new_sn_f65 <int>, new_ep_m014 <int>,
    ## #   new_ep_m1524 <int>, new_ep_m2534 <int>, new_ep_m3544 <int>,
    ## #   new_ep_m4554 <int>, new_ep_m5564 <int>, new_ep_m65 <int>,
    ## #   new_ep_f014 <int>, new_ep_f1524 <int>, new_ep_f2534 <int>,
    ## #   new_ep_f3544 <int>, new_ep_f4554 <int>, new_ep_f5564 <int>,
    ## #   new_ep_f65 <int>, newrel_m014 <int>, newrel_m1524 <int>,
    ## #   newrel_m2534 <int>, newrel_m3544 <int>, newrel_m4554 <int>,
    ## #   newrel_m5564 <int>, newrel_m65 <int>, newrel_f014 <int>,
    ## #   newrel_f1524 <int>, newrel_f2534 <int>, newrel_f3544 <int>,
    ## #   newrel_f4554 <int>, newrel_f5564 <int>, newrel_f65 <int>

who 자료 중 filter()를 이용하여 2006\~2010 자료만을 뽑아 NewWho에 저장했습니다.

### 2\. NewWho자료를 아래의 기준에 따라 tidy data로 변형하여 NewwhoTidy에 저장하시오.

  - type: TB type으로 변수이름의 두 번째 부분 (sp, rel, sn, ep)

  - gender: 성별

  - ageL, ageU: age group의 하한과 상한을 의미하며 65세 이상 그룹에서는 ageU를 100으로 함

  - age: ageL과 ageU의 평균 값

<!-- end list -->

``` r
NewwhoTidy <- NewWho %>% 
  gather(new_sp_m014:newrel_f65, key = "key", value = "value", na.rm = TRUE) %>% 
  mutate(key = stringr::str_replace(key, "newrel", "new_rel")) %>% 
  separate(key, c("new", "type", "sexage"), sep = "_") %>% 
  select(-new, -iso2, -iso3) %>% 
  separate(sexage, c("gender", "age_group"), sep = 1) %>%
  mutate(age_group = ifelse(age_group==65, 6500, age_group)) %>%
  separate(age_group, c("ageL", "ageU"), sep = -2, convert = TRUE) %>%
  mutate(ageU = ifelse(ageU==00, 100, ageU)) %>%
  mutate(age = (ageL+ageU)/2 ) %>%
  select(country, year, type, gender, ageL, age, ageU, value)
NewwhoTidy
```

    ## # A tibble: 32,512 x 8
    ##    country      year type  gender  ageL   age  ageU value
    ##    <chr>       <int> <chr> <chr>  <int> <dbl> <dbl> <int>
    ##  1 Afghanistan  2006 sp    m          0     7    14   193
    ##  2 Afghanistan  2007 sp    m          0     7    14   186
    ##  3 Afghanistan  2008 sp    m          0     7    14   187
    ##  4 Afghanistan  2009 sp    m          0     7    14   200
    ##  5 Afghanistan  2010 sp    m          0     7    14   197
    ##  6 Albania      2006 sp    m          0     7    14     5
    ##  7 Albania      2007 sp    m          0     7    14     0
    ##  8 Albania      2008 sp    m          0     7    14     1
    ##  9 Albania      2009 sp    m          0     7    14     0
    ## 10 Albania      2010 sp    m          0     7    14     0
    ## # ... with 32,502 more rows

먼저 key를 gather()한 뒤, newrel을 new\_rel로 변환합니다. 그리고 key를 separate() 함수를
이용하여 분리한 뒤, 필요없는 변수 new, iso2, iso3 제거합니다. sexage를 gender와
age\_group으로 분리하여, age\_group이 65인 경우 네 자리의 6500으로 변환합니다. 그리고
age\_group을 ageL과 ageU로 분리하고, ageU가 00인 경우 65세 이상 그룹의 상한이므로 ageU가 100이
되도록 변환합니다. ageL과 ageU의 평균 값인 ‘age’ 변수를 추가했으며, 최종 NewwhoTidy 자료에는
country, year, type, gender, ageL, age, ageU, value만 나타나도록 하였습니다.

``` r
head(NewwhoTidy)
```

    ## # A tibble: 6 x 8
    ##   country      year type  gender  ageL   age  ageU value
    ##   <chr>       <int> <chr> <chr>  <int> <dbl> <dbl> <int>
    ## 1 Afghanistan  2006 sp    m          0     7    14   193
    ## 2 Afghanistan  2007 sp    m          0     7    14   186
    ## 3 Afghanistan  2008 sp    m          0     7    14   187
    ## 4 Afghanistan  2009 sp    m          0     7    14   200
    ## 5 Afghanistan  2010 sp    m          0     7    14   197
    ## 6 Albania      2006 sp    m          0     7    14     5

``` r
tail(NewwhoTidy)
```

    ## # A tibble: 6 x 8
    ##   country   year type  gender  ageL   age  ageU value
    ##   <chr>    <int> <chr> <chr>  <int> <dbl> <dbl> <int>
    ## 1 Yemen     2010 ep    f         65  82.5   100     0
    ## 2 Zimbabwe  2006 ep    f         65  82.5   100    92
    ## 3 Zimbabwe  2007 ep    f         65  82.5   100     9
    ## 4 Zimbabwe  2008 ep    f         65  82.5   100   104
    ## 5 Zimbabwe  2009 ep    f         65  82.5   100   138
    ## 6 Zimbabwe  2010 ep    f         65  82.5   100   146

### 3\. 2에서 만든 NewWhoTidy 자료를 이용하여 나라별, type 별, 연도별로 TB 수의 합을 구하고 각 연도별, type별로 구한 TB 수가 가장 큰 나라를 찾으시오.

``` r
NewwhoTidy %>%
  group_by(country) %>%
  summarize(country_value=sum(value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 213 x 2
    ##    country             country_value
    ##    <chr>                       <int>
    ##  1 Afghanistan                 64268
    ##  2 Albania                      2122
    ##  3 Algeria                     41182
    ##  4 American Samoa                 18
    ##  5 Andorra                        36
    ##  6 Angola                     109117
    ##  7 Anguilla                        1
    ##  8 Antigua and Barbuda            16
    ##  9 Argentina                   40191
    ## 10 Armenia                      7094
    ## # ... with 203 more rows

NewWhoTidy 자료를 이용한 나라별 TB 수의 합입니다.

``` r
NewwhoTidy %>%
  group_by(type) %>%
  summarize(type_value=sum(value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 3 x 2
    ##   type  type_value
    ##   <chr>      <int>
    ## 1 ep       1397403
    ## 2 sn       3987357
    ## 3 sp      12804721

NewWhoTidy 자료를 이용한 type별 TB 수의 합입니다.

type에는 아래와 같이 real, ep, sn, sp로 총 4가지가 있으나, 이 자료에서 재발 건수를 뜻하는 rel 값은
NA이기 때문에 나타나지 않습니다.

  - rel :cases of relapse

  - ep : cases of extrapulmonary TB

  - sn : cases of pulmonary TB that could not be diagnosed by a
    pulmonary smear (smear negative)

  - sp : cases of pulmonary TB that could be diagnosed be a pulmonary
    smear (smear positive)

<!-- end list -->

``` r
NewwhoTidy %>%
  group_by(year) %>%
  summarize(year_value=sum(value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 5 x 2
    ##    year year_value
    ##   <int>      <int>
    ## 1  2006    2996524
    ## 2  2007    3836674
    ## 3  2008    3535278
    ## 4  2009    3834194
    ## 5  2010    3986811

NewWhoTidy 자료를 이용한 연도별 TB 수의 합입니다.

  - 연도별로 구한 TB 수가 가장 큰 나라 찾기

### 2006년

``` r
NewwhoTidy %>% 
  filter(year==2006) %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 197 x 2
    ##    country                          sum_value
    ##    <chr>                                <int>
    ##  1 India                               553797
    ##  2 China                               468291
    ##  3 South Africa                        272162
    ##  4 Indonesia                           175320
    ##  5 Bangladesh                          140948
    ##  6 Philippines                          85740
    ##  7 Brazil                               74358
    ##  8 Pakistan                             65308
    ##  9 Democratic Republic of the Congo     63144
    ## 10 Viet Nam                             56437
    ## # ... with 187 more rows

2006년 TB 수가 가장 큰 나라는 India입니다.

### 2007년

``` r
NewwhoTidy %>% 
  filter(year==2007) %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 189 x 2
    ##    country                          sum_value
    ##    <chr>                                <int>
    ##  1 India                              1198289
    ##  2 China                               465877
    ##  3 South Africa                        286973
    ##  4 Indonesia                           271278
    ##  5 Russian Federation                  120912
    ##  6 Bangladesh                          104296
    ##  7 Pakistan                             88747
    ##  8 Philippines                          86566
    ##  9 Brazil                               71827
    ## 10 Democratic Republic of the Congo     66099
    ## # ... with 179 more rows

2007년 TB 수가 가장 큰 나라는 India입니다.

### 2008년

``` r
NewwhoTidy %>% 
  filter(year==2008) %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 194 x 2
    ##    country            sum_value
    ##    <chr>                  <int>
    ##  1 India                 615492
    ##  2 China                 462596
    ##  3 South Africa          320026
    ##  4 Indonesia             292899
    ##  5 Bangladesh            187471
    ##  6 Russian Federation    120835
    ##  7 Pakistan              100037
    ##  8 Philippines            85025
    ##  9 Kenya                  78697
    ## 10 Brazil                 70484
    ## # ... with 184 more rows

2008년 TB 수가 가장 큰 나라는 India입니다.

### 2009년

``` r
NewwhoTidy %>% 
  filter(year==2009) %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 196 x 2
    ##    country                               sum_value
    ##    <chr>                                     <int>
    ##  1 China                                    884477
    ##  2 India                                    624617
    ##  3 South Africa                             383774
    ##  4 Indonesia                                289044
    ##  5 Russian Federation                       117227
    ##  6 Pakistan                                 101674
    ##  7 Philippines                               88806
    ##  8 Kenya                                     87427
    ##  9 Democratic People's Republic of Korea     74089
    ## 10 Brazil                                    71572
    ## # ... with 186 more rows

2009년 TB 수가 가장 큰 나라는 China입니다.

### 2010년

``` r
NewwhoTidy %>% 
  filter(year==2010) %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 204 x 2
    ##    country                               sum_value
    ##    <chr>                                     <int>
    ##  1 China                                    869092
    ##  2 India                                    630164
    ##  3 South Africa                             364992
    ##  4 Indonesia                                296272
    ##  5 Bangladesh                               150856
    ##  6 Pakistan                                 124455
    ##  7 Russian Federation                       102823
    ##  8 Philippines                               89198
    ##  9 Kenya                                     85484
    ## 10 Democratic People's Republic of Korea     81240
    ## # ... with 194 more rows

2010년 TB 수가 가장 큰 나라는 China입니다.

  - type별로 구한 TB 수가 가장 큰 나라 찾기

### ep

``` r
NewwhoTidy %>% 
  filter(type=="ep") %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 189 x 2
    ##    country                               sum_value
    ##    <chr>                                     <int>
    ##  1 South Africa                             302720
    ##  2 India                                    206840
    ##  3 Bangladesh                                64054
    ##  4 Morocco                                   59837
    ##  5 Brazil                                    51344
    ##  6 Kenya                                     47691
    ##  7 Indonesia                                 40595
    ##  8 United Republic of Tanzania               39904
    ##  9 Russian Federation                        37273
    ## 10 Democratic People's Republic of Korea     36861
    ## # ... with 179 more rows

“ep” type의 TB 수가 가장 큰 나라는 South Africa입니다.

### sn

``` r
NewwhoTidy %>% 
  filter(type=="sn") %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 188 x 2
    ##    country                               sum_value
    ##    <chr>                                     <int>
    ##  1 China                                    868193
    ##  2 South Africa                             648124
    ##  3 Indonesia                                429326
    ##  4 India                                    398862
    ##  5 Russian Federation                       292705
    ##  6 Zimbabwe                                 116085
    ##  7 Brazil                                   113401
    ##  8 Bangladesh                               104578
    ##  9 Democratic People's Republic of Korea    100220
    ## 10 Kenya                                     93444
    ## # ... with 178 more rows

“sn” type의 TB 수가 가장 큰 나라는 China입니다.

### sp

``` r
NewwhoTidy %>% 
  filter(type=="sp") %>% 
  group_by(country) %>%
  summarize(sum_value=sum(value)) %>%
  arrange(desc(sum_value))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 212 x 2
    ##    country                          sum_value
    ##    <chr>                                <int>
    ##  1 India                              3016657
    ##  2 China                              2275815
    ##  3 Indonesia                           854892
    ##  4 South Africa                        677083
    ##  5 Pakistan                            460507
    ##  6 Philippines                         435335
    ##  7 Bangladesh                          418405
    ##  8 Democratic Republic of the Congo    342862
    ##  9 Viet Nam                            267960
    ## 10 Nigeria                             220224
    ## # ... with 202 more rows

“sp” type의 TB 수가 가장 큰 나라는 India입니다.
