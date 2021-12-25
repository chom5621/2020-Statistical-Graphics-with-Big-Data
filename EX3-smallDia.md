HW3
================
Jo, Munsun
2020 9 23

# 파일 읽어오기

``` r
getwd()
```

    ## [1] "C:/Temp"

``` r
setwd("C:/Temp")
smallDia <- read.csv("smallDia.csv", header=T)
head(smallDia)
```

    ##   carat       cut color clarity depth table price
    ## 1  0.72   Premium     I      IF  63.0    57  2795
    ## 2  0.60 Very Good     G      IF  61.6    56  2800
    ## 3  0.56 Very Good     E      IF  61.0    59  2833
    ## 4  0.58 Very Good     E      IF  60.6    59  2852
    ## 5  0.62      Fair     F      IF  60.1    61  2861
    ## 6  0.63   Premium     E      IF  60.3    62  2879

getwd()를 통해 현재 작업디렉토리를 확인하고, setwd()를 통해 작업디렉토리를 “C:/Temp”로 지정했습니다.

read.csv()를 이용하여 “smallDia.csv” 파일을 smallDia에 읽어왔고, header를 별도로 인식하기 위해
header=T를 사용했습니다.

head(smallDia)를 통해 7개의 변수 carat, cut, color, clarity, depth, table,
price를 확인했습니다.

그 결과 carat(무게)과 depth(깊이), table(윗면 넓이), price(가격)는 연속변수로,

cut(cutting의 질), color(색), clarity(투명도)는 범주형 변수로 볼 수 있었습니다.

# 1\. 연속변수 carat(무게) 통계량과 그래프

carat(무게)은 산술연산이 가능한 변수이기에 양적 변수인 연속변수로 보았습니다.

``` r
min(smallDia$carat)
```

    ## [1] 0.23

``` r
max(smallDia$carat)
```

    ## [1] 2.29

``` r
quantile(smallDia$carat)
```

    ##   0%  25%  50%  75% 100% 
    ## 0.23 0.32 0.41 1.00 2.29

``` r
mean(smallDia$carat)
```

    ## [1] 0.6101384

``` r
# summary(smallDia$carat)을 통해서도 Min, 1st Qu, Median, Mean, 3rd Qu, Max 확인 가능
sd(smallDia$carat)
```

    ## [1] 0.371483

최솟값을 알려주는 min()함수에 따라 min=0.23입니다.

최댓값을 알려주는 max()함수에 따라 max=2.29입니다.

최솟값, Q1, 중앙값, Q3, 최댓값을 계산하는 quantile()함수에 따라 Q1=0.32, Q2=0.41,
Q3=1.00입니다.

평균을 알려주는 mean()함수에 따라 mean=0.6101384입니다.

표준편차를 알려주는 sd()함수에 따라 sd=0.371483입니다.

통계량을 통해 이 자료에서 다이아몬드는 0.23캐럿부터 2.29캐럿까지 존재하며, 평균은 약 0.61캐럿이고 표준편차는 약
0.37캐럿임을 알 수 있습니다. 그리고 하위 25% 값은 0.32캐럿, 중위수는 0.41캐럿, 75% 값은 1.00캐럿임을 알
수 있습니다. 즉 평균이 0.61캐럿으로 중위수인 0.41캐럿보다 크므로, 오른쪽으로 꼬리가 긴 분포를 예상할 수 있습니다.

``` r
library(ggplot2)
ggplot(smallDia,aes(carat))+geom_histogram(binwidth=0.1)
```

![](EX3-smallDia_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

히스토그램은 다이아몬드의 carat 값을 0.1 간격으로 나눠 각 구간 내 데이터 값의 빈도를 막대로 나타냅니다. carat
변수는 봉우리가 3개인 분포로, 0.3 근처에서 가장 높은 빈도를 가지고 있습니다. 그리고 그래프 맨 오른쪽에 캐럿이
약 2.3인 특이치를 식별할 수 있습니다.

# 2\. 범주형 변수 cut(cutting의 질) 통계량과 그래프

cut은 Fair, Good, Very Good, Premium, Ideal 다섯 등급으로 나뉘되 Ideal이 가장 좋은
등급이라고 합니다. 즉 cut(cutting의 질)은 산술연산이 불가능한 변수이기에 명목 변수인 범주형 변수로
보았습니다.

``` r
table(smallDia$cut)
```

    ## 
    ##      Fair      Good   Premium Very Good 
    ##         9        71       230       268

SmallDia에서 cut은 Fair, Good, Very Good, Premium으로 총 4개의 범주가 있음을 알 수 있습니다.
각 범주별 도수를 구하자면, Fair가 9개, Good이 71개, Very Good이 268개, Premium이 230개임을
table()함수를 통해 알 수 있습니다. 이를 통해 가장 좋은 등급인 Ideal은 없지만 그 다음으로 좋은 Premium과
Very Good이 대부분이며, Very Good, Premium, Good, Fair 순임을 수치상 알 수 있습니다.

``` r
ggplot(smallDia,aes(cut))+geom_bar()
```

![](EX3-smallDia_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

하나의 범주형 변수 cut을 표현하는 Bar 그래프를 통해 각 범주의 도수를 시각적으로 쉽게 파악할 수 있습니다. smallDia
자료에서는 cut이 Very Good인 다이아몬드가 제일 많으며, Premium인 다이아몬드도 그에 이어 많은 것을 그래프를 통해
알 수 있습니다. 그에 비해 상대적으로 Good과 Fair인 다이아몬드는 Good, Fair 순으로 적음을 시각적으로 쉽게 알 수
있습니다.

# 3\. 범주형 변수 color(색) 통계량과 그래프

color는 D, E, F, G, H, I, J의 7등급으로 나뉘는데, D가 가장 좋은 등급이라고 합니다. 즉 color(색)은
산술연산이 불가능한 변수이기에 명목 변수인 범주형 변수로 보았습니다.

``` r
table(smallDia$color)
```

    ## 
    ##   D   E   F   G   H   I   J 
    ##  45  79 117 190  73  48  26

SmallDia에서 color는 D, E, F, G, H, I, J로 총 4개의 범주가 있음을 알 수 있습니다. 각 범주별 도수를
구하자면, D는 45개, E는 79개, F는 117개, G는 190개, H는 73개, I는 48개, J는 26개임을
table()함수를 통해 알 수 있습니다. 이를 통해 중간 등급인 G등급이 가장 많고, 그 다음으로는 F, E, H, I,
D, J등급 순임을 수치상으로 알 수 있습니다.

``` r
ggplot(smallDia,aes(color))+geom_bar()
```

![](EX3-smallDia_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

하나의 범주형 변수 color를 표현하는 Bar 그래프를 통해 각 범주의 도수를 시각적으로 쉽게 파악할 수 있습니다.
smallDia 자료에서는 color가 G등급인 다이아몬드가 제일 많으며, 그에 이어 F, E, H, I, D, J등급 순이라는
것을 그래프를 통해 한눈에 알 수 있습니다.

# 4\. 범주형 변수 clarity(투명도) 통계량과 그래프

clarity는 I1, SI1, SI2, VS1,VS2, VVS1, VVS2, IF의 8등급으로 나누고 IF가 가장 좋은
등급이라고 합니다. 즉 clarity(투명도)은 산술연산이 불가능한 변수이기에 명목 변수인 범주형 변수로
보았습니다.

``` r
table(smallDia$clarity)
```

    ## 
    ##  IF 
    ## 578

SmallDia에서 clarity는 IF 범주 하나만 있음을 알 수 있습니다. 그래서 범주별 도수를 구하자면, IF 등급이
578개임을 table()함수를 통해 알 수 있습니다. 이를 통해 이 자료의 모든 다이아몬드는 투명도가 가장 좋은 등급인
IF라는 것을 수치상으로 알 수 있습니다.

``` r
ggplot(smallDia,aes(clarity))+geom_bar()
```

![](EX3-smallDia_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

하나의 범주형 변수 clarity를 표현하는 Bar 그래프를 통해 smallDia 자료에서는 범주가 IF등급 하나임을 시각적으로
쉽게 파악할 수 있습니다.

# 5\. 연속변수 depth(깊이) 통계량과 그래프

depth(깊이)는 산술연산이 가능한 변수이기에 양적 변수인 연속변수로 보았습니다.

``` r
min(smallDia$depth)
```

    ## [1] 52.3

``` r
max(smallDia$depth)
```

    ## [1] 65.6

``` r
quantile(smallDia$depth)
```

    ##   0%  25%  50%  75% 100% 
    ## 52.3 60.2 61.3 62.3 65.6

``` r
mean(smallDia$depth)
```

    ## [1] 61.22734

``` r
# summary(smallDia$depth)를 통해서도 Min, 1st Qu, Median, Mean, 3rd Qu, Max 확인 가능
sd(smallDia$depth)
```

    ## [1] 1.554369

최솟값을 알려주는 min()함수를 통해 min=52.3입니다.

최댓값을 알려주는 max()함수를 통해 max=65.6입니다.

최솟값, Q1, 중앙값, Q3, 최댓값을 계산하는 quantile()함수에 따라 Q1=60.2, Q2=61.3,
Q3=62.3입니다.

평균을 알려주는 mean()함수를 통해 mean=61.22734입니다.

표준편차를 알려주는 sd()함수를 통해 sd=1.554369입니다.

통계량을 통해 이 자료에서 다이아몬드의 깊이는 52.3부터 65.6까지 존재하며, 평균은 약 61.2이고 표준편차는 약
1.55임을 알 수 있습니다. 그리고 하위 25% 값은 60.2, 중위수는 평균과 비슷한 61.3, 75% 값은
62.3임을 알 수 있습니다.

``` r
ggplot(smallDia,aes(depth))+geom_histogram(binwidth=0.5)
```

![](EX3-smallDia_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

히스토그램은 다이아몬드의 depth 값을 0.5 간격으로 나눠 각 구간 내 데이터 값의 빈도를 막대로 나타냅니다. 그래프를 통해
깊이가 약 61인 다이아몬드가 가장 많은 것을 알 수 있습니다. 이를 중심으로 depth 변수는 봉우리가 1 개인 종모양의
대칭 분포입니다. 그리고 그래프 맨 왼쪽에 깊이가 약 52.5인 특이치를 식별할 수 있습니다.

# 6\. 연속변수 table(윗면 넓이) 통계량과 그래프

table(윗면 넓이)은 산술연산이 가능한 변수이기에 양적 변수인 연속변수로 보았습니다.

``` r
min(smallDia$table)
```

    ## [1] 53

``` r
max(smallDia$table)
```

    ## [1] 65

``` r
quantile(smallDia$table)
```

    ##   0%  25%  50%  75% 100% 
    ##   53   57   58   59   65

``` r
mean(smallDia$table)
```

    ## [1] 58.18512

``` r
# summary(smallDia$table)을 통해서도 Min, 1st Qu, Median, Mean, 3rd Qu, Max 확인 가능
sd(smallDia$table)
```

    ## [1] 2.105202

최솟값을 알려주는 min()함수를 통해 min=53입니다.

최댓값을 알려주는 max()함수를 통해 max=65입니다.

최솟값, Q1, 중앙값, Q3, 최댓값을 계산하는 quantile()함수에 따라 Q1=57, Q2=58, Q3=59입니다.

평균을 알려주는 mean()함수를 통해 mean=58.18512입니다.

표준편차를 알려주는 sd()함수를 통해 sd=2.105202입니다.

통계량을 통해 이 자료에서 다이아몬드의 윗면 넓이는 53부터 65까지 존재하며, 평균은 약 58.2이고 표준편차는 약 2.1임을
알 수 있습니다. 그리고 하위 25% 값은 57, 중위수는 평균과 비슷한 58, 75% 값은 59임을 알 수 있습니다.

``` r
ggplot(smallDia,aes(table))+geom_histogram(binwidth=1)
```

![](EX3-smallDia_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

히스토그램은 다이아몬드의 table 값을 1 간격으로 나눠 각 구간 내 데이터 값의 빈도를 막대로 나타냅니다. 그래프를 통해 윗면
넓이가 58인 다이아몬드가 가장 많은 것을 알 수 있습니다. 따라서 table 변수는 정수값을 가지며 가운데 봉우리가 58로 가장
높지만, 57의 빈도값이 확연히 낮아 종 모양은 아닌 분포입니다.

# 7\. 연속변수 price(가격) 통계량과 그래프

price(가격)은 산술연산이 가능한 변수이기에 양적 변수인 연속변수로 보았습니다.

``` r
min(smallDia$price)
```

    ## [1] 369

``` r
max(smallDia$price)
```

    ## [1] 18594

``` r
quantile(smallDia$price)
```

    ##      0%     25%     50%     75%    100% 
    ##   369.0   954.0  1333.0  7072.5 18594.0

``` r
mean(smallDia$price)
```

    ## [1] 4106.04

``` r
# summary(smallDia$price)를 통해서도 Min, 1st Qu, Median, Mean, 3rd Qu, Max 확인 가능
sd(smallDia$price)
```

    ## [1] 4714.359

최솟값을 알려주는 min()함수를 통해 min=369입니다.

최댓값을 알려주는 max()함수를 통해 max=18594입니다.

최솟값, Q1, 중앙값, Q3, 최댓값을 계산하는 quantile()함수에 따라 Q1=954.0, Q2=1333.0,
Q3=7072.5입니다.

평균을 알려주는 mean()함수를 통해 mean=4106.04입니다.

표준편차를 알려주는 sd()함수를 통해 sd=4714.359입니다.

통계량을 통해 이 자료에서 다이아몬드의 가격은 369부터 18594까지 존재하며, 평균은 약 4106이고 표준편차는 약
4714임을 알 수 있습니다. 그리고 하위 25% 값은 954, 중위수는 1333, 75% 값은 7072.5임을 알 수
있습니다. 즉 평균이 중위수보다 확연히 크므로, 오른쪽으로 꼬리가 긴 분포를 예상할 수 있습니다.

``` r
ggplot(smallDia,aes(price))+geom_histogram(binwidth=1000)
```

![](EX3-smallDia_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

히스토그램은 다이아몬드의 price 값을 1000 간격으로 나눠 각 구간 내 데이터 값의 빈도를 막대로 나타냅니다. 그래프를 통해
가격이 약 1000인 다이아몬드가 가장 많은 것을 알 수 있습니다. price 변수는 오른쪽으로 꼬리가 긴 형태의 분포로, 그래프
구간이 0부터 20000까지 있고 가격이 1000 이외에는 고루 분포하는 것을 보아 산포가 큽니다.
