# 6장 확률 플룻의 활용

-  연속형 자료의 경우 특정 확률모형을 상징하는 경우 그 모형과 관측자료의 부합하는 지 평가를 위해 qq(Quantile-quantile) 플롯 이용

#### QQ plot
- 두 표본 데이터의 분포가 서로 동일한지 알아보는 시각적 도구
- 동일한 분포인 경우 직선

## 6.1 정규확률 분포


```r
x <- seq(-3,3,0.01)
str(x)
```

```
##  num [1:601] -3 -2.99 -2.98 -2.97 -2.96 -2.95 -2.94 -2.93 -2.92 -2.91 ...
```

```r
x[590:601]
```

```
##  [1] 2.89 2.90 2.91 2.92 2.93 2.94 2.95 2.96 2.97 2.98 2.99 3.00
```

```r
# 밀도함수
y<-dnorm(x) # 확률밀도함수 공식 적용
plot(y ~ x, type="l", ylim=c(0,0.5), ylab="density")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-11.png) 

```r
# 분위수 함수
p <- (seq(1,500)-0.5) / 500  # 분위수 함수 계산식 (책 p105)
z <- qnorm(p, mean=0, sd=1)
plot(z ~ p, type="l")
abline(c(0,0), lty="dotted")  # 직선 형태가 되어야 하는데 아님 (직선의 절편: 평균, 기울기:표준편차)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-12.png) 

```r
# 예를 들어보면, Darwin의 옥수수종 Zea Mays실험자료

# 방법 2 : R의 정규확률플롯 함수 qnorm()이용
darwin <- c(49, -67, 8, 16, 6, 23, 28, 41, 14, 29, 56, 24, 75, 60, -48)
qqnorm(darwin)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-13.png) 

```r
# 방법 1 : 수평축에 표준정규분포 N(0,1)로부터 자료 길이의 분위수 z를 넣고, 수직축에는 자료를 정렬하여 넣는 방법
p <- (1:length(darwin)-0.5)/ length(darwin)
z <- qnorm(p)
plot(sort(darwin) ~ z, type = "l", ylim=c(-75, 75), xlim=c(-2,2), main="Darwin")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-14.png) 

```r
x11(); qqnorm(darwin, ylim=c(-75,75), xlim=c(-2,2)) # 편집 윈도우창
```

```
## Warning: unable to open connection to X11 display ''
```

```
## Error: unable to start device X11cairo
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-15.png) 
- 특이점(outliers) 2개 or 정규분포를 따르지 않는다.(특이점이라면 빼고 본다)

## 6.2 각종 임의수의 모의생성 방법
- 실제 관측자료의 통계적 모형화에 도움


### 1) 균일분포로부터의 모의생성

```r
runif(10)   # 0~1까지의 임의수
```

```
##  [1] 0.2750 0.4292 0.1028 0.9177 0.3661 0.2014 0.1714 0.8547 0.8453 0.4373
```

```r
runif(10,-1,1)
```

```
##  [1] -0.6699 -0.5747  0.8462 -0.6339 -0.6901  0.4420 -0.6563  0.3659
##  [9]  0.6890 -0.6842
```

```r
trunc(runif(25,0,10))   # 소수점 버림
```

```
##  [1] 1 2 6 2 8 4 4 3 0 6 0 8 7 5 0 2 5 3 4 5 6 7 1 0 3
```


### 2) 정규분포로부터의 모의 생성
- 평균이 같고 분산이 각기 다른 분포
- 밀도함수 : 평균과 표준편차로 정규분포를 그릴 수 있는  함수
- 표준정규분포 : 평균 0, 표준편차 1 


```r
rnorm(10)   # N(0,1)
```

```
##  [1]  0.430902  1.482639 -1.368694 -1.559513 -0.179923  0.007255  0.956897
##  [8] -0.780654  2.024850 -0.850648
```

```r
rnorm(10,5,2.5)    #N(5, 2.5)
```

```
##  [1] 1.366 1.420 5.398 5.315 5.470 6.850 4.151 8.544 4.276 8.148
```

```r
# 표준정규분포에서 1이상의 확률 구할 때 활용
z <- rnorm(10000, 0, 1)
sum(z>1) / 10000
```

```
## [1] 0.1579
```

```r
#표준정규분포에서 확률의 경우
1-pnorm(1)   
```

```
## [1] 0.1587
```


### 3) 지수분포로부터의 모의생성
- 어떤 사건이 일어나는 시간 간격의 분화와 관계가 있다.
- 연속확률분포의 일종
- 사건이 서로 독립적일 때, 일정시간 동안 발생하는 사건의 횟수는 프아송 분포
- 다음 사건이 일어날 때까지 대시시간은 지수분포


```r
rexp(10)
```

```
##  [1] 0.6564 0.5234 0.6726 0.7204 0.5114 0.1287 0.5188 0.1148 0.9049 2.5529
```

```r
rexp(10, 0.5)
```

```
##  [1] 0.28073 2.80212 2.13727 6.53458 3.02047 2.82923 0.02135 2.25042
##  [9] 0.30275 0.92871
```

```r
x <- rexp(10000)
mean(x)
```

```
## [1] 1.005
```

```r
x.plus <- x[x > 1] -1  #more waiting time after waiting 1 min.
mean(x.plus)
```

```
## [1] 0.9852
```

### 4) 베르누이 분포와 이항분포로부터의 모의생성
- 베르누이 분포 :성공확률이 p인 사건의 성공과 실패를 모형화한 이산분포
- 이항분포 : 성공확률이 p인 사건의 n회 독립시행에서 성공의 수


```r
# Bernoulli
rbinom(25, 1, 0.5)
```

```
##  [1] 1 1 1 1 1 0 0 1 0 0 0 1 0 0 0 1 1 1 1 0 0 0 0 0 0
```

```r
x <- rbinom(10001, 1, 0.5)
x.1 <- x[2:10001]
x.0 <- x[1:10000]
addmargins(table(x.0, x.1))
```

```
##      x.1
## x.0       0     1   Sum
##   0    2610  2501  5111
##   1    2501  2388  4889
##   Sum  5111  4889 10000
```

```r
#Binomial
x <- rbinom(10000, 10, 0.5)
addmargins(table(x))
```

```
## x
##     0     1     2     3     4     5     6     7     8     9    10   Sum 
##     6    91   434  1159  2127  2462  2059  1126   431    98     7 10000
```

```r
dbinom(10,10,0.5)
```

```
## [1] 0.0009766
```


### 5)프아송(Poisson) 분포
- 사건이 서로 독립적일 때, 일정시간 동안 발생하는 사건의 횟수


```r
rpois(25,1)
```

```
##  [1] 3 0 1 0 3 0 1 1 3 0 1 1 1 1 2 1 1 1 1 0 2 2 1 0 1
```

```r
hist(rpois(1000,1), breaks = seq(-0.5, 10.5,1))
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-61.png) 

```r
hist(rpois(1000,2), breaks = seq(-0.5, 10.5,1))
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-62.png) 

### 6)그 밖의 분포 (책참조)


## 6.3 정규확률 플롯의 여러 패턴

```r
# 1)정규분포로부터의 모의생성 자료에 대한 정규확률 플롯
# 예) IQ test의 평균 100, 표준편차 15가 되도록 한 후, 40명 표본추출한 경우는,

par(mfrow = c(1,2))
qqnorm(rnorm(40, 100, 15))
qqnorm(rnorm(40, 100, 15))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-71.png) 

```r
# 2) 혼합정규분포로부터의 모의생성 자료에 대한 정규확률 플롯
# 예) 평균이 70/표준편차 15일 정규분포로부터 20명 추출한 것과
# 평균이 130 / 표준편차가 15인 분포로부터 20명 임의추출한 것에서

qqnorm(c(rnorm(20,70,15), rnorm(20,130,15)))
qqnorm(c(rnorm(20,70,15), rnorm(20,130,15)))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-72.png) 

```r
# 3) 특이값이 내재하는 모의생성 자료에 대한 정규확률 플롯
# 예) 38개의 자료점에서 평균이 100, 표준편차 15인 정규분포에서 2개의 특이값 25와 175를 입력시켜 40개의 표본자료 만듬

qqnorm(c(25,175, rnorm(38, 100,15)))
qqnorm(c(25,175, rnorm(38, 100,15)))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-73.png) 

```r
# 4) 꼬리가 짧은 분포로부터 모의생성 자료에 대한 정규확률 플롯

qqnorm(runif(40,80,120))   # 80 ~ 120 사이에서 40개
qqnorm(runif(40,80,120))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-74.png) 

```r
# 5) 꼬리가 긴 분포로부터의 모의생성 자료에 대한 정규확률 플롯
# 꼬리가 긴 분포의 예인 이중지수분포로부터 표본자료를 임의 생성
qqnorm(c(rexp(20,1), -rexp(20,1))) # lamda = 1인 20개의 지수분포의 모의수
qqnorm(c(rexp(20,1), -rexp(20,1)))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-75.png) 

```r
# 6) 큰 값 쪽으로 긴 꼬리를 뻗은 기울어진 분포의 경우
# 기울어진 분포의 예는 로그정규분포가 있다

qqnorm(exp(rnorm(40,5,1)))   # 평균은 5, 표준편차 1
qqnorm(exp(rnorm(40,5,1)))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-76.png) 

```r
# 작은 값 쪽으로 긴 꼬리를 뻗은 기울어진 분포의 경우
# 6번의 반대를 위해 값을 뺀다

qqnorm(1500 - exp(rnorm(40,5,1)))
qqnorm(1500 - exp(rnorm(40,5,1)))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-77.png) 

## 6.4 지수분포와 와이블 분포로부터의 확률 플롯
- 수명(lifetime:생존시간) 자료에 대해서는 지수분포와 와이블 분포를 많이 사용
- 지수분포 : 어떤 사건이 일어나는 시간간격의 분포와 관계
- 와이블 분포 : 스웨덴 물리학자 와이블이 만듬. 재료의 파괴강도를 분석하면서 고안함. 전자 및 기계부품의 수명분포를 나타내는 데에 적합
- 지수분포를 포함한 분포군으로서 수명시간에 대한 확률모형으로 유용하다



```r
# 백혈병 환자 21명의 생존시간

#leukemia <- scan() # read data values
leukemia <- c(1,1,2,2,3,4,4,5,5,8,8,8,8,11,11,12,12,15,17,22,23)
n <- length(leukemia)
p <- seq(1:n)/n - 0.5/n
x <- -log(1-p)
y <- leukemia
plot(y ~ x, main = "Q-Q plot for exponential dist")

#지수분포의 예 (약간의 곡선 : 적합치 않다)
library(lattice)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-81.png) 

```r
qqmath(~leukemia, distribution = function(p) qexp(p,1))   # lamda=1 인 지수분포를 이용
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-82.png) 

```r
# 와이블 분포의 예 (직선적 경향선 : 적합)
qqmath(~leukemia, distribution =function(p) qweibull(p,1.5,1))   # a (shape)=1.5 , b(기울기) = 1
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-83.png) 

```r
# 와이블 분포의 최대가능도 적합 (maximum likelihood fitting)
library(MASS)
fitdistr(leukemia, "weibull")  # 괄호안의 숫자가 추정치에 대한 표준오차이다. 형태(shape)모수 a가 1.37로 a=1.5와 유사 
```

```
##    shape    scale 
##   1.3705   9.4818 
##  (0.2379) (1.5911)
```

## 6.5 두 표본의 등화
- 두 표본 데이터의 분포를 비교하여 차이를 없애는 것
- X형 시험지, Y형 시험지의 난이도가 다르면 안되므로 두 표본 데이터의 분포를 일치시킨다. QQ plot을 통해 변환함수를 찾는다.


```r
x <- rbeta(800, 2,3)*100   # 베타 분포 / rbeta(n, shape1, shape2) / shape1, shape2 : positive parameters of the Beta distribution
y <- rbeta(1200, 3,2)*100

# X형과 Y형의 중간값이 차이가 크다. 그래서 X형을 Y형 값으로 올려줌

round(quantile(x), 1) #decimal place is 0.1
```

```
##   0%  25%  50%  75% 100% 
##  2.1 23.8 37.6 54.4 94.0
```

```r
round(quantile(y), 1)
```

```
##   0%  25%  50%  75% 100% 
##  2.1 44.8 62.4 75.7 99.1
```

```r
# QQ plot이 변환함수 그래프를 만들어 줌

par(mfrow=c(1,2))
qqplot(x, y, xlim=c(0,100))
qqplot(x, y, xlim=c(0,100), type="l")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-91.png) 

```r
# 두 분위서의 백분위수가 일치하게 됨

# 표본 데이터의 크기가 작은 경우는? (등백분위수 등화가 아닌, 등사분위수 등화를 적용)

x <- rbeta(80, 2,3)*100
x <- rbeta(120, 2,3)*100
x.quantile <- round(quantile(x), 1)
y.quantile <- round(quantile(y), 1)
par(mfrow=c(1,2))
qqplot(x,y, xlim=c(0,100), type = "l")
x.quantile[1] <- 0
x.quantile[5] <- 100
y.quantile[1] <- 0
y.quantile[5] <- 100
plot(y.quantile ~ x.quantile, xlim=c(0,100), type="l")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-92.png) 
