HW5
================
Jo, Munsun
2020 10 12

## 1\. mpg 자료의 displ과 hwy를 이용하여 아래의 그림을 그리기 위한 code를 작성하시오. 분수를 나타내기 위해 frac(miles, gallon)를 이용하면 됨.

``` r
library(ggplot2)
head(mpg)
```

    ## # A tibble: 6 x 11
    ##   manufacturer model displ  year   cyl trans      drv     cty   hwy fl    class 
    ##   <chr>        <chr> <dbl> <int> <int> <chr>      <chr> <int> <int> <chr> <chr> 
    ## 1 audi         a4      1.8  1999     4 auto(l5)   f        18    29 p     compa~
    ## 2 audi         a4      1.8  1999     4 manual(m5) f        21    29 p     compa~
    ## 3 audi         a4      2    2008     4 manual(m6) f        20    31 p     compa~
    ## 4 audi         a4      2    2008     4 auto(av)   f        21    30 p     compa~
    ## 5 audi         a4      2.8  1999     6 auto(l5)   f        16    26 p     compa~
    ## 6 audi         a4      2.8  1999     6 manual(m5) f        18    26 p     compa~

``` r
ggplot(data=mpg, aes(displ, hwy)) + geom_point() + labs(x="Displacement", y=expression( "Highway(" * frac(miles,gallon) * ")" )) + scale_x_continuous(breaks = c(2,3,4,5,6,7), labels = paste0(c(2,3,4,5,6,7),"L"))
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

mpg 자료의 displ과 hwy를 이용하여 Displacement(배기량)별 Highway(고속도로연비)의 분포를 산점도로
확인할 수 있었습니다. 그림를 보면 displ 변수와 hwy 변수 간에 음의 상관관계가 느슨하게 나오는 것을 발견할 수
있습니다. 다시 말해 엔진이 큰 차일수록 연비가 낮다고 짐작할 수 있습니다.

## 2\. 아래의 code를 실행시키고 문제점을 파악한 후 문제점을 수정한 그림을 그리시오.

``` r
ggplot(mpg, aes(displ, hwy)) +
 geom_point(aes(colour = drv, shape = drv)) 
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

문제점: 그래프 상 겹치는 점들에 있어 drv를 제대로 파악하기 어렵다는 문제점이 있습니다.

``` r
ggplot(mpg, aes(displ, hwy)) +
 geom_point(aes(colour = drv, shape = drv), alpha=0.5) 
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

문제점 수정 방법1: 그래프가 겹치는 문제점을 해결하기 위해 투명도(alpha)를 추가했습니다. drv가 겹치는 부분을 구별하기
위해 alpha = 0.5 로 하여 반투명하게 하였습니다.

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point(aes(colour = drv, shape = drv), alpha=0.5) + facet_wrap(~drv)
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

문제점 수정 방법2: 나아가 facet을 이용하여 범주 drv의 각 그룹별로 나눠서 표현하였습니다. 마찬가지로 drv는
colour와 shape을 통해 구분하였고, alpha = 0.5 로 하여 반투명하게 하였습니다.

``` r
ggplot(mpg, aes(displ, hwy)) +
  geom_bin2d(aes(colour = drv), alpha=0.5) 
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

문제점 수정 방법3: 2차원 binning을 이용하여 점이 겹쳐지는 정도를 농도로 나타내 보았습니다. drv는 colour를 통해
구분하였고, alpha = 0.5 로 하여 반투명하게 하였습니다.

## 3\. mpg 자료에서 drv 별로 cyl와 hwy의 분포를 알아보려고 한다. 이를 위한 다양한 그림을 그리고 설명하시오.

``` r
ggplot(mpg, aes(cyl, hwy)) + geom_point(aes(colour = drv, shape = drv), alpha=0.5)
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

전반적으로 cyl이 증가할수록 hwy는 감소하는 추세를 확인할 수 있습니다. 즉 실린더 수(cyl)가 클수록 연비(hwy)는
감소한다는 것을 짐작할 수 있습니다.

``` r
ggplot(mpg, aes(cyl, hwy)) + geom_point(aes(colour = drv, shape = drv), alpha=0.5) + facet_wrap(~drv)
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

facet을 이용하여 범주 drv의 각 그룹별(4, f, r)로 나눠서 표현하였습니다. 또한 drv는 colour와 shape을
통해 구분하였고, alpha = 0.5 로 하여 반투명하게 하였습니다.

평균적으로 drv(구동방식)이 f일 때 hwy(연비)가 가장 좋고, 4일 때와 r일 때는 hwy(연비)가 떨어진단 것을 확인할 수
있습니다.

``` r
ggplot(mpg, aes(factor(cyl), hwy)) + geom_boxplot(aes(colour = drv, shape = drv), alpha=0.5) + facet_wrap(~drv) + xlab("cyl")
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

평균적으로 drv(구동방식)이 f일 때 cyl(실린더 수)가 가장 적은 편이고, r일 때 cyl(실린더 수)가 가장 많다는 것을
확인할 수 있습니다.

이를 통해 drv가 f일 때는 cyl가 적으므로 hwy가 높은 반면, r일 때는 cyl가 크므로 hwy가 낮은 것을 확인할 수
있습니다. 한편 drv가 4일 때는 cyl가 고루 분포하나 hwy는 r과 비슷하기 때문에, 4라는 구동방식도 연비에 영향을
미친다는 것을 짐작할 수 있었습니다.

``` r
ggplot(data = mpg) + geom_bin2d(aes(x = cyl, y = hwy, color = drv), alpha=0.5)
```

![](EX5-mpg_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

점이 겹쳐지는 정도를 농도로 나타내는 2차원 binning을 통해서도, 전반적으로 cyl이 증가할수록 hwy는 감소하는 추세를
확인할 수 있습니다.

## 4\. mpg 자료의 displ(x변수)와 hwy(y변수)의 산점도에서 class 별로 선형회귀직선을 그려 아래와 같은 그림을 얻고자 한다. 이에 맞는 code를 작성하시오.

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point(aes(group=class, colour=class)) + geom_smooth(aes(group=class, colour=class), method="lm", se=FALSE)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](EX5-mpg_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

displ와 hwy의 산점도를 통해 전반적으로 displ가 증가할수록 hwy는 감소한다는 것을 확인할 수 있습니다. 또한
class 별로 선형회귀직선을 그려, 각 class별 displ와 hwy의 관계를 파악할 수 있습니다.
