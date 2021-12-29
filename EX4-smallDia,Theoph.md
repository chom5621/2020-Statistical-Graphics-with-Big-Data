HW4
================
Jo, Munsun
2020 10 2

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

carat(무게)과 depth(깊이), table(윗면 넓이), price(가격)는 연속변수로, cut(cutting의 질),
color(색), clarity(투명도)는 범주형 변수로 볼 수 있습니다.

# 1\. 다이아몬드 가격에 가장 큰 영향을 미치는 변수 찾기

## 1\) carat과 price의 관계

두 개의 연속변수 carat, price의 관계를 알아보기 위해 아래와 같은 산점도를 그렸습니다.

``` r
library(ggplot2)
# Create scatterplot, 'geom_point()': 산점도 그리기
ggplot(smallDia, aes(x=carat, y=price))+geom_point()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->
다이아몬드의 무게인 carat이 증가함에 따라 price, 가격이 증가한다는 사실을 산점도를 통해 대략적으로 예측할 수
있습니다.

``` r
# Jittering, 'geom_jitter()': 자료에 약간의 랜덤 노이즈를 더하여 겹쳐져서 그려지지 않게 하는 방법
ggplot(smallDia, aes(x=carat, y=price))+geom_jitter()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
데이터 값을 조금씩 움직여 점이 겹치지 않도록 위 산점도를 다시 그려보았습니다.

``` r
# 'geom_smooth(method="lm")': 선형 회귀분석 추세선 출력
ggplot(smallDia, aes(x=carat, y=price))+geom_point()+geom_smooth(method="lm")
```

    ## `geom_smooth()` using formula 'y ~ x'

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->
carat에 따른 price 값을 나타낸 산점도에 선형 회귀분석 추세선을 더했습니다. 그리고 파란색 추세선 양옆에
표준오차(standard error)를 회색으로 같이 표시했습니다. 추세선이 일직선으로 위와 같이 나타난 것을 보아
carat과 price 간에 선형관계가 있다는 것을 짐작할 수 있습니다. (carat이 증가할수록 price가 증가합니다.)

``` r
# 회귀식 및 R-squared 값 찾기
summary(lm(price~carat, smallDia))
```

    ## 
    ## Call:
    ## lm(formula = price ~ carat, data = smallDia)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -5480.6  -836.8   -12.4   403.9  9474.8 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  -2935.7      157.0  -18.69   <2e-16 ***
    ## carat        11541.2      219.9   52.48   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1962 on 576 degrees of freedom
    ## Multiple R-squared:  0.8271, Adjusted R-squared:  0.8268 
    ## F-statistic:  2755 on 1 and 576 DF,  p-value: < 2.2e-16

회귀식을 ’y=11541.2x-2935.7’처럼 쓸 수 있으며

p-value 값은 0.05보다 작으므로 독립변수 carat이 반응변수인 price를 설명하는데 유의하다고 볼 수 있습니다.

결정계수는 ’R²=0.8271’로 1에 가까우므로, 추정 모형의 정확도(설명력)는 높은 편입니다.

## 2\) cut와 price의 관계

범주형 변수 cut, 연속변수 price의 관계를 알아보기 위해 아래와 같은 산점도, 상자그림, 밀도함수, 히스토그램을
그렸습니다.

``` r
# Create scatterplot, 'geom_point()': 산점도 그리기
ggplot(smallDia, aes(x=cut, y=price))+geom_point()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
다이아몬드 cutting의 질인 cut이 Fair일 때 price는 5000 이하에 불과하지만, cut이 Good,
Premium, Very Good일 때 price는 0\~20000 사이에 고루 분포합니다. 등급이 높아짐에 따라 높은
price를 갖는 경우도 생기는 것을 알 수 있습니다.

``` r
# Jittering, 'geom_jitter()': 자료에 약간의 랜덤 노이즈를 더하여 겹쳐져서 그려지지 않게 하는 방법
ggplot(smallDia, aes(x=cut, y=price))+geom_jitter()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->
데이터 값을 조금씩 움직여 점이 겹치지 않도록 위 산점도를 다시 그려보았습니다.

그 결과 Fair, Good, Premium, Very Good 순으로 자료가 많아진다는 것을 알 수 있으며,

각 범주마다 price가 0에 가까운 자료가 제일 많고 price가 높아질수록 자료의 개수가 점점 줄어든다는 것을 알 수
있습니다.

``` r
# Boxplots, 'geom_boxplot()': summary statistics 를 이용하여 상자그림을 그리는 방법
ggplot(smallDia, aes(x=cut, y=price))+geom_boxplot()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
cut별로 price에 대한 상자그림을 그렸습니다. 상자그림을 통해 각 범주의 중앙값을 비교할 수 있습니다.(price 중앙값:
Premium \< Very Good \< Good \< Fair)

만약 cut이 price에 영향을 미친다면 여기서 cutting의 질이 가장 높은 Premium 등급이나 그 다음인 Very
Good 등급에서 price가 높게 나타나야 할 텐데, 상자그림을 보면 오히려 그 반대로 Premium에서 price 중앙값이
가장 낮게 나타납니다.

``` r
# 'geom_density()'
ggplot(smallDia, aes(price, color=cut))+geom_density()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->
cut별 price 밀도함수를 그렸습니다.

만약 cut이 price에 영향을 미친다면 여기서 cutting의 질이 가장 낮은 Fair 등급이 price가 낮은 곳에서
density가 가장 높은 것은 맞지만, 오히려 가장 높은 등급인 Premium이 price가 낮은 곳에서 density가
그다음으로 높게 나옵니다.

``` r
# 'geom_histogram() + facet_wrap()'
ggplot(smallDia, aes(price))+geom_histogram(aes(fill=cut))+facet_wrap(~cut,ncol=2)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
cut별 price 히스토그램을 그렸습니다.

만약 cut이 price에 영향을 미친다면 여기서 cutting의 질이 가장 높은 Premium 등급이나 그 다음인 Very
Good 등급에서 price가 높게 나타나야 할 텐데, 히스토그램을 보면 오히려 그 반대로 Premium과 Very Good에서
price가 가장 낮은 0\~2000의 빈도수가 가장 높게 나타납니다.

이를 통해 cut은 price에 가장 큰 영향을 미치는 변수가 아니며, 다른 변수로 인해 (ex. cut이 높은 등급일수록
carat이 적다든지) 이런 결과가 나타난 것이라 볼 수 있습니다.

## 3\) color와 price의 관계

범주형 변수 color, 연속변수 price의 관계를 알아보기 위해 아래와 같은 산점도, 상자그림, 밀도함수, 히스토그램을
그렸습니다.

``` r
# Create scatterplot, 'geom_point()': 산점도 그리기
ggplot(smallDia, aes(x=color, y=price))+geom_point()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
다이아몬드의 색인 color이 D, E, F, G, H, I, J일 때 모두 price가 0\~20000 사이에 고루 분포한다는
것을 산점도를 통해 대략적으로 예측할 수 있습니다.

``` r
# Jittering, 'geom_jitter()': 자료에 약간의 랜덤 노이즈를 더하여 겹쳐져서 그려지지 않게 하는 방법
ggplot(smallDia, aes(x=color, y=price))+geom_jitter()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
데이터 값을 조금씩 움직여 점이 겹치지 않도록 위 산점도를 다시 그려보았습니다.

그 결과 각 범주마다 price가 0에 가까운 자료가 제일 많고 price가 높아질수록 자료의 개수가 점점 줄어든다는 것을 알 수
있습니다.

``` r
# Boxplots, 'geom_boxplot()': summary statistics 를 이용하여 상자그림을 그리는 방법
ggplot(smallDia, aes(x=color, y=price))+geom_boxplot()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->
color별로 price에 대한 상자그림을 그렸습니다. 상자그림을 통해 각 범주의 price 중앙값을 비교할 수 있습니다.

만약 color가 price에 영향을 미친다면 color가 가장 좋은 등급인 D에서 price의 중앙값이 가장 높게 나타나는 것은
맞지만, 가장 낮은 등급인 J에서 price 중앙값이 두 번째로 큰 것이 나타나며 나머지 등급의 price 중앙값은 거의 비슷하게
낮게 나타납니다.

``` r
# 'geom_density()'
ggplot(smallDia, aes(price, color=color))+geom_density()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->
color별 price 밀도함수를 그렸습니다.

color 등급이 가장 낮은 J가 아니라, 오히려 중간 등급인 H가 price가 낮은 곳에서 density가 가장 높게 나옵니다.

``` r
# 'geom_histogram() + facet_wrap()'
ggplot(smallDia, aes(price))+geom_histogram(aes(fill=color))+facet_wrap(~color,ncol=2)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
color별 price 히스토그램을 그렸습니다.

color의 가장 좋은 등급인 D에서 높은 price를 갖는 관측치가 보이지만, H와 I에서는 price가 15,000을 넘는
관측치가 없습니다. G에서는 상대적으로 빈도수가 높은데, 낮은 price를 갖는 관측치가 많아 보입니다.

color에 따라 price가 다른 양상을 보이지만 뚜렷한 규칙성은 없는 것으로 보아 가장 큰 영향을 미치는 변수가 아니며,
영향력이 더 큰 다른 변수가 있기에 이런 결과가 나타난 것이라고 봅니다.

## 4\) clarity와 price의 관계

범주형 변수 clarity, 연속변수 price의 관계를 알아보기 위해 아래와 같은 산점도, 상자그림, 밀도함수, 히스토그램을
그렸습니다.

``` r
# Create scatterplot, 'geom_point()': 산점도 그리기
ggplot(smallDia, aes(x=clarity, y=price))+geom_point()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->
이 자료에서 다이아몬드의 투명도인 clarity는 IF 하나만 존재하여, 산점도에 따라 price는 0\~20000 사이에
분포하고 있습니다.

``` r
# Jittering, 'geom_jitter()': 자료에 약간의 랜덤 노이즈를 더하여 겹쳐져서 그려지지 않게 하는 방법
ggplot(smallDia, aes(x=clarity, y=price))+geom_jitter()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->
데이터 값을 조금씩 움직여 점이 겹치지 않도록 위 산점도를 다시 그려보았습니다.

IF 범주에서 price가 0에 가까운 자료가 제일 많고 price가 높아질수록 자료의 개수가 점점 줄어든다는 것을 알 수
있습니다.

``` r
# Boxplots, 'geom_boxplot()': summary statistics 를 이용하여 상자그림을 그리는 방법
ggplot(smallDia, aes(x=clarity, y=price))+geom_boxplot()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->
clarity별로 price에 대한 상자그림을 그렸습니다.

``` r
# 'geom_density()'
ggplot(smallDia, aes(price, color=clarity))+geom_density()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->
clarity별 price 밀도함수를 그렸습니다.

``` r
# 'geom_histogram() + facet_wrap()'
ggplot(smallDia, aes(price))+geom_histogram()+facet_wrap(~clarity)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
clarity에 따른 price 히스토그램을 그렸습니다.

산점도와 상자그림, 밀도함수, 히스토그램에 의하면 price 빈도수는 0\~200이 많고 또 2000까지 퍼져있기 때문에
clarity의 단일변수인 IF가 price에 큰 영향을 미치지는 않는 것으로 보입니다.

이를 통해 clarity은 price에 가장 큰 영향을 미치는 변수가 아니며, 영향력이 더 큰 다른 변수가 있기에 이런 결과가
나타난 것이라고 봅니다.

## 5\) depth와 price의 관계

두 개의 연속변수 depth, price의 관계를 알아보기 위해 아래와 같은 산점도를 그렸습니다.

``` r
# Create scatterplot, 'geom_point()': 산점도 그리기
ggplot(smallDia, aes(x=depth, y=price))+geom_point()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->
다이아몬드의 깊이인 depth는 대부분 58\~64 사이이며, price가 0에 가까운 자료가 제일 많고 price가 높아질수록
자료의 개수가 점점 줄어든다는 것을 알 수 있습니다.

일정한 패턴없이 점이 퍼져있어 depth와 price의 관계를 발견하기 어렵습니다.

``` r
# Jittering, 'geom_jitter()': 자료에 약간의 랜덤 노이즈를 더하여 겹쳐져서 그려지지 않게 하는 방법
ggplot(smallDia, aes(x=depth, y=price))+geom_jitter()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->
데이터 값을 조금씩 움직여 점이 겹치지 않도록 위 산점도를 다시 그려보았습니다.

``` r
# 'geom_smooth(method="lm")': 선형 회귀분석 추세선 출력
ggplot(smallDia, aes(x=depth, y=price))+geom_point()+geom_smooth(method="lm")
```

    ## `geom_smooth()` using formula 'y ~ x'

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->
depth에 따른 price 값을 나타낸 산점도에 선형 회귀분석 추세선을 더했습니다. 그리고 파란색 추세선 양옆에
표준오차(standard error)를 회색으로 같이 표시했습니다. 산점도와 추세선이 위와 같이 나타난 것을 보아
depth과 price 간에 선형관계가 보이지 않습니다.

``` r
# 회귀식 및 R-squared 값 찾기
summary(lm(price~depth, smallDia))
```

    ## 
    ## Call:
    ## lm(formula = price ~ depth, data = smallDia)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ##  -4214  -3171  -2636   2947  14464 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)  -4577.7     7731.6  -0.592    0.554
    ## depth          141.8      126.2   1.124    0.262
    ## 
    ## Residual standard error: 4713 on 576 degrees of freedom
    ## Multiple R-squared:  0.002187,   Adjusted R-squared:  0.0004543 
    ## F-statistic: 1.262 on 1 and 576 DF,  p-value: 0.2617

회귀식을 ’y=141.8x-4577.7’처럼 쓸 수 있으며

p-value 값은 0.2617로 0.05보다 크므로 독립변수인 depth는 반응변수인 price를 설명하는데 유의하다고 볼 수
없습니다.

결정계수는 ’R²=0.002187’로 0에 가까우므로, 추정 모형의 정확도(설명력)는 낮습니다.

## 6\) table과 price의 관계

두 개의 연속변수 table, price의 관계를 알아보기 위해 아래와 같은 산점도를 그렸습니다.

``` r
# Create scatterplot, 'geom_point()': 산점도 그리기
ggplot(smallDia, aes(x=table, y=price))+geom_point()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->
일정한 패턴없이 점이 퍼져있어 table과 price의 관계를 발견하기 어렵습니다.

``` r
# Jittering, 'geom_jitter()': 자료에 약간의 랜덤 노이즈를 더하여 겹쳐져서 그려지지 않게 하는 방법
ggplot(smallDia, aes(x=table, y=price))+geom_jitter()
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->
데이터 값을 조금씩 움직여 점이 겹치지 않도록 위 산점도를 다시 그려보았습니다.

``` r
# 'geom_smooth(method="lm")': 선형 회귀분석 추세선 출력
ggplot(smallDia, aes(x=table, y=price))+geom_point()+geom_smooth(method="lm")
```

    ## `geom_smooth()` using formula 'y ~ x'

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->
table에 따른 price 값을 나타낸 산점도에 선형 회귀분석 추세선을 더했습니다. 그리고 파란색 추세선 양옆에
표준오차(standard error)를 회색으로 같이 표시했습니다. 산점도와 추세선이 위와 같이 나타난 것을 보아
table과 price 간에 선형관계가 보이지 않습니다.

``` r
# 회귀식 및 R-squared 값 찾기
summary(lm(price~table, smallDia))
```

    ## 
    ## Call:
    ## lm(formula = price ~ table, data = smallDia)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ##  -5230  -3120  -2331   2971  14999 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept) -10895.10    5396.54  -2.019  0.04396 * 
    ## table          257.82      92.69   2.782  0.00559 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 4687 on 576 degrees of freedom
    ## Multiple R-squared:  0.01325,    Adjusted R-squared:  0.01154 
    ## F-statistic: 7.737 on 1 and 576 DF,  p-value: 0.005587

회귀식을 ’y=257.82x-10895.10’처럼 쓸 수 있으며

p-value 값은 0.05보다 작으므로 독립변수인 table은 반응변수인 price를 설명하는데 유의하다고 볼 수 있습니다.

결정계수는 ’R²=0.01325’로 0에 가까우므로, 추정 모형의 정확도(설명력)는 낮습니다.

## 7\) 다이아몬드 price 결정에 영향을 미치는 변수에 대한 결론

다이아몬드 price 결정에 가장 큰 영향을 미치는 변수는 carat(무게)일 것입니다.

위의 해석 결과에 따르면 연속변수 carat만 산점도와 회귀식 추세선에 의해 carat이 증가하면 price도 증가하며,
p-value 값과 결정계수에 따르면 carat이 price를 설명하는 데 유의하고 그 설명력은 높다고 할 수 있었습니다.

다른 변수 depth, table도 price에 영향을 미치며 cut, color도 어느정도 price 결정에 영향을 줄
것입니다.

# 2\. R에 내장되어 있는 Theoph 자료는 12명의 사람으로부터 시간별로 측정한 약물의 혈중농도에 관한 자료이다.

``` r
data(Theoph)
head(Theoph)
```

    ## Grouped Data: conc ~ Time | Subject
    ##   Subject   Wt Dose Time  conc
    ## 1       1 79.6 4.02 0.00  0.74
    ## 2       1 79.6 4.02 0.25  2.84
    ## 3       1 79.6 4.02 0.57  6.57
    ## 4       1 79.6 4.02 1.12 10.50
    ## 5       1 79.6 4.02 2.02  9.66
    ## 6       1 79.6 4.02 3.82  8.58

``` r
tail(Theoph)
```

    ## Grouped Data: conc ~ Time | Subject
    ##     Subject   Wt Dose  Time conc
    ## 127      12 60.5  5.3  3.52 9.75
    ## 128      12 60.5  5.3  5.07 8.57
    ## 129      12 60.5  5.3  7.07 6.59
    ## 130      12 60.5  5.3  9.03 6.11
    ## 131      12 60.5  5.3 12.05 4.57
    ## 132      12 60.5  5.3 24.15 1.17

``` r
class(Theoph)
```

    ## [1] "nfnGroupedData" "nfGroupedData"  "groupedData"    "data.frame"

### \- Subject : 12명의 ID

### \- Wt : 몸무게

### \- Dose : 복용양

### \- Time : 시간

### \- conc : 혈중농도

## 12명의 시간에 따른 혈중농도 변화곡선

``` r
library(ggplot2)
ggplot(Theoph,aes(x=Time,y=conc,col=Subject))+geom_point()+geom_line()+facet_wrap(~Subject)
```

![](EX4-smallDia,Theoph_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->
12명의 시간에 따른 혈중농도 변화곡선을 geom\_line()을 이용하여 위와 같은 시계열 그래프로 나타냈습니다. 또한
facet\_wrap()을 이용하여 Subject 범주별(1\~12) Time에 대한 conc 그래프들을 하나의 그림에
표현하였습니다.

그 결과 12명마다 미세하게는 다르지만 대부분 Time이 0\~5인 사이 혈중농도 conc가 급격히 증가했다가, 이후에는 시간이
지남에 따라 혈중농도도 점점 감소하는 경향을 띠고 있습니다.

Subject가 6인 사람이 혈중농도의 peak가 가장 낮은데, 이 그래프를 왼쪽 위에서부터 시작하여 peak 오름차순으로
그래프가 그려지다가, 혈중농도의 peak가 가장 높은 Subject 5의 그래프로 오른쪽 아래 끝이 납니다.
