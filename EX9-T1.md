HW9
================
Jo, Munsun
2020 11 26

### 1\. T1을 tibble 함수를 이용하여 만드시오.

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
T1 <- tibble(
  school = c("A","A","A","B","B","B"), # school=rep(LETTERS[1:2], each=3)
  grade = c(1,2,3,1,2,3), # grade=rep(1:3, times=2)
  Female = c(15,30,45,60,75,90), # Female=(1:6)*15
  Male = c(70,140,210,280,350,420) # Male=(1:6)*70
)
T1$grade <- as.integer(T1$grade)
T1
```

    ## # A tibble: 6 x 4
    ##   school grade Female  Male
    ##   <chr>  <int>  <dbl> <dbl>
    ## 1 A          1     15    70
    ## 2 A          2     30   140
    ## 3 A          3     45   210
    ## 4 B          1     60   280
    ## 5 B          2     75   350
    ## 6 B          3     90   420

### 2\. T1은 tidy data인가? (답에 대한 이유도 함께 쓰시오)

T1은 tidy data입니다.

이유:

tidy data의 다음 세 가지 조건을 만족하므로, T1은 tidy data입니다.

1.  Each variable must have its own column.

각 변수는 개별의 열(column)로 존재합니다.

2.  Each observation must have its own row.

각 관측치는 행(row)을 구성합니다.

3.  Each value mush have its own cell.

각 테이블은 하나의 관측기준에 의해서 조직된 데이터를 저장합니다.

### 3\. T1을 이용하여 T2, T3, T41, T42를 만드시오 (단, tibble 함수는 사용하지 말것)

``` r
T2 <- T1 %>% gather(`Female`, `Male`, key = "gender", value = "n")
T2
```

    ## # A tibble: 12 x 4
    ##    school grade gender     n
    ##    <chr>  <int> <chr>  <dbl>
    ##  1 A          1 Female    15
    ##  2 A          2 Female    30
    ##  3 A          3 Female    45
    ##  4 B          1 Female    60
    ##  5 B          2 Female    75
    ##  6 B          3 Female    90
    ##  7 A          1 Male      70
    ##  8 A          2 Male     140
    ##  9 A          3 Male     210
    ## 10 B          1 Male     280
    ## 11 B          2 Male     350
    ## 12 B          3 Male     420

gather()함수를 이용하여 `Female`과 `Male`을 “gender”라는 새로운 column에 합쳐 나타냈으며, 그에
따른 학생수를 value = “n” column으로 나타냈습니다.

``` r
T3 <- T1 %>% unite(FM, Female, Male, sep="*")
T3
```

    ## # A tibble: 6 x 3
    ##   school grade FM    
    ##   <chr>  <int> <chr> 
    ## 1 A          1 15*70 
    ## 2 A          2 30*140
    ## 3 A          3 45*210
    ## 4 B          1 60*280
    ## 5 B          2 75*350
    ## 6 B          3 90*420

unite()함수를 이용하여 `Female`과 `Male`을 "\*“로 붙인”FM"이라는 새로운 column에 나타냈습니다.

``` r
T41 <- T1 %>% select(-Male) %>% spread(grade, Female) 
T41
```

    ## # A tibble: 2 x 4
    ##   school   `1`   `2`   `3`
    ##   <chr>  <dbl> <dbl> <dbl>
    ## 1 A         15    30    45
    ## 2 B         60    75    90

select()함수를 이용하여 `Male` 값을 제외하고, spread()함수를 이용하여 “grade”별 A,B학교의
`Female` 수를 각각 나타냈습니다.

``` r
T42 <- T1 %>% select(-Female) %>% spread(grade, Male) 
T42
```

    ## # A tibble: 2 x 4
    ##   school   `1`   `2`   `3`
    ##   <chr>  <dbl> <dbl> <dbl>
    ## 1 A         70   140   210
    ## 2 B        280   350   420

select()함수를 이용하여 `Female` 값을 제외하고, spread()함수를 이용하여 “grade”별 A,B학교의
`Male` 수를 각각 나타냈습니다.

### 4\. T2를 T1의 형태로 만드시오.

``` r
T2 %>% spread(key=gender, value=n)
```

    ## # A tibble: 6 x 4
    ##   school grade Female  Male
    ##   <chr>  <int>  <dbl> <dbl>
    ## 1 A          1     15    70
    ## 2 A          2     30   140
    ## 3 A          3     45   210
    ## 4 B          1     60   280
    ## 5 B          2     75   350
    ## 6 B          3     90   420

spread()함수를 이용하여 “gender” column의 `Female`, `Male`을 T1처럼 각각 따로 나타냈습니다.

### 5\. T3을 T1의 형태로 만드시오.

``` r
T3 %>% separate(FM, into = c("Female", "Male"))
```

    ## # A tibble: 6 x 4
    ##   school grade Female Male 
    ##   <chr>  <int> <chr>  <chr>
    ## 1 A          1 15     70   
    ## 2 A          2 30     140  
    ## 3 A          3 45     210  
    ## 4 B          1 60     280  
    ## 5 B          2 75     350  
    ## 6 B          3 90     420

separate()함수를 이용하여 “gender” column의 `Female`, `Male`을 T1처럼 각각 따로 나타냈습니다.

### 6\. T41과 T42를 이용하여 T1의 형태를 만드시오.

``` r
T41 %>% gather(`1`, `2`, `3`, key = "grade", value = "Female") %>% left_join(T42 %>% gather(`1`, `2`, `3`, key = "grade", value = "Male")) %>% arrange(school)
```

    ## Joining, by = c("school", "grade")

    ## # A tibble: 6 x 4
    ##   school grade Female  Male
    ##   <chr>  <chr>  <dbl> <dbl>
    ## 1 A      1         15    70
    ## 2 A      2         30   140
    ## 3 A      3         45   210
    ## 4 B      1         60   280
    ## 5 B      2         75   350
    ## 6 B      3         90   420

T41과 T42를 각각 T1 형태처럼 만들어 left\_join하였고, T1처럼 “school” column으로 정렬하여
나타냈습니다.

### 7\. 각 학교별, 학년별로 총 학생수를 구하여 Ttot를 만드시오.

``` r
Ttot <- T1 %>% mutate(total=Female+Male) %>% select(-Female, -Male) %>% group_by(school) %>% spread(grade, total) 
Ttot
```

    ## # A tibble: 2 x 4
    ## # Groups:   school [2]
    ##   school   `1`   `2`   `3`
    ##   <chr>  <dbl> <dbl> <dbl>
    ## 1 A         85   170   255
    ## 2 B        340   425   510

mutate()함수를 이용하여 Female과 Male을 합친 total(총 학생수) 변수를 생성하였고, school을 그룹지정하여
row에는 각 학교별, column에는 각 학년별로 총 학생수를 나타냈습니다.
