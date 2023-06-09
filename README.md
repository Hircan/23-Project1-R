# 23-Project1-R
R언어 기반 실무 프로젝트

# 12주차 수업내용

## 정렬

>정렬 : 주어진 기준에 따라 데이터를 크기순으로 재배열하는 과정

### 벡터의 정렬

```R
v1 <- c(1,7,6,8,4,2,3)
v1 <- sort(v1) # 오름차순
v2 <- sort(v1, decreasing=T) # 내림차순
결과
v1 : [1] 1 2 3 4 6 7 8
v2 : [1] 8 7 6 4 3 2 1
```

문자열도 정렬이 가능하고 한글의 경우 가나다 순으로 정렬이 됨
NA가 포함된 벡터를 정렬하면 오름차순 내림차순 상관없이 NA 값들은 맨 뒤쪽에 위치함

`sort()`함수는 값에 크기에 따라 값들을 정렬
`order()`함수는 값의 크기에 따라 값들의 인덱스를 정렬

```R
name <- c('정대일','강재구','신현석','홍길동')
order(name) # 오름챃순
order(name, decreasing=T) # 내림차순

idx <- order(name)
name[idx] # 오름차순 정렬
결과
idx : [1] 2 3 1 4
name[idx] : [1] "강재구" "신현석" "정대일" "홍길동"
```

### 매트릭스와 데이터프레임의 정렬

```R
head(iris)
order(iris$Sepal.Length)
iris[order(iris$Sepal.Length),] # 오름차순 정렬
iris[order(iris$Sepal.Length, decreasing=1),] # 내림차순 정렬
iris.new <- iris[order(iris$Sepal.Length),] # 정렬된 데이터 저장
head(iris.new)
iris[order(iris$Specties, decreasing=T, iris$Petal.Length),] # 정렬 기준이 2개
```

## 샘플링
> 샘플링 : 주어진 값들이 있을 때 그중에서 임의의 개수만큼 값들을 추출하는 작업

> 비복원 추출 : 한 번 뽑은 값은 제외한 뒤 새로 추출

> 복원 추출 : 뽑았던 값을 다시 포함시켜 새로 추출

**샘플링이 필요할 때 : 데이터셋이 너무 커 분석에 시간이 많이 걸리는 경우, 일부의 데이터만 추출하여 대략의 결과를 미리 확인**

```R
# 샘플링 예시
idx <- sample(1:nrow(iris), size=50, replace=F)
iris.50 <- iris
```

## 조합

> 조합(combination) : 주어진 데이터값 중에서 몇 개씩 짝을 지어 추출하는 작업으로, `combn()`함수를 사용함

```R
# 조합 예시
combn(1:5,3)  # 1~5에서 3개를 뽑는 조합
x <- c("red","green","blue","black","white")
com <-  combn(x,2)

for(i in 1:ncol(com)){
  cat(com[,1], '\n')
}
```

## 집계

> 집계(aggregation) : 데이터의 그룹에 대해서 합계나 평균을 계산하는 작업

```R
# 집계 예시
agg <- aggregate(iris[,-5], by=list(품종=iris$Species),FUN=mean)

결과
        품종 Sepal.Length Sepal.Width Petal.Length Petal.Width
1     setosa        5.006       3.428        1.462       0.246
2 versicolor        5.936       2.770        4.260       1.326
3  virginica        6.588       2.974        5.552       2.026

```
## 고급 그래프 작성하기

### 나무지도 작성방법

```R
install.packages('treemap')
library(treemap)
data(GNI2014)
head(GNI2014)
treemap(GNI2014,
        index = c('continent', 'iso3'), # 계층 구조
        vSize = 'population', # 타일 크기
        vColor= 'GNI', # 타일 컬러
        type = 'value', # 타일컬러링방법
        # bg.labels = "220", # 레이블배경색
        title = "World's GNI") # 제목
```
**버그인지 bg.labels를 이용하면 오류가 남 오픈소스가 변경된 듯한데 검색해도 잘 안나오니 배경색은 빼도록 하자**

# 11주차 수업내용
## 다중변수 데이터 분석
### 상관관계
```R
# 데이터 확인
head(pressure)

# 산점도 작성
plot(pressure$temperature,    # x축 데이터
     pressure$pressure,       # y축 데이터
     main='온도와 기압',      # 그래프 제목
     xlab='온도(화씨)',       # x축 레이블
     ylab='기압',             # y축 레이블
)

# 상관계수 확인
cor(pressure$temperature, pressure$pressure)
결과값 : [1] 0.7577923
```
상관계수는 -1 부터 1의 값을 가지는데,
-1에 가까울 수록 음의 상관관계(반비례)
1에 가까울 수록 양의 상관관계(비례)
0인 경우는 두 변수 사이에 상관성을 찾기 어렵다는 걸 뜻한다.

데이터프레임을 이용해 다중변수 사이의 상관관계를 나타낼 수도 있음
```R
# 데이터 확인
st <- data.frame(state.x77)
head(st)

# 다중 산점도 작성
plot(st)

# 다중 상관계수
cor(st)
```
---

## 결측값에 대해 알아보자

결측값을 처리하는 방법 2가지
- 결측값을 제거하거나 제외한 후 분석
- 결측값을 추정하여 적당한 값으로 치환한 후 분석

### 벡터의 결측값
```R
z <- c(1,2,3,NA,5,NA,8)  # 결측값이 포함된 벡터 z
sum(z)                   # 정상 계산이 되지 않음
결과값 : [1] NA
is.na(z)                 # NA 여부 확인
결과값 : [1] FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE
sum(is.na(z))            # NA 개수 확인
결과값 : [1] 2
sum(z, na.rm=TRUE)       # NA를 제외하고 합계를 계산
결과값 : [1] 19
```

```R
z1 <- c(1,2,3,NA,5,NA,8)
z2 <- c(5,6,1,NA,3,NA,7)
z1[is.na(z1)] <- 0 # NA 0으로 치환 TRUE인 인덱스만 가져오니까
z3 <- as.vector(na.omit(z2)) # NA 제거하고 새로운 백터 생성
```
**na.omit()함수는 NA를 제거하는 함수인데, 결과의 자료구조가 벡터가 아니기 때문에 as.vector() 함수를 이용하여 벡터로 만든다.**

### 매트릭스와 데이터프레임의 결측값

```R
# NA 포함하는 테스트 데이터
x <- iris
x[1,2] <- NA; x[1,3] <- NA
x[2,3] <- NA; x[3,4] <- NA
head(x)

# 열별로 결측값이 몇 개인지 확인하는 방법 (for문)
for(i in 1:ncol(x)){
    this.na <- is.na(x[i])
    cat(colnames(x)[i], '\t', sum(this.na), '\n')
}

# 열별로 결측값이 몇 개인지 확인하는 방법 (apply)
col_na <- function(y){
  return(sum(is.na(y)))
}
na_count <- apply(x, 2, FUN=col_na)
na_count

# 행별로 결측값이 몇 개인지 확인하는 방법
rowSums(is.na(x))
sum(rowSums(is.na(x))>0)
sum(is.na(x))

# 결측값 제외하고 새로운 데이터셋 만드는 방법(complete.cases())
head(x)
x[!complete.cases(x),]        # NA가 포함된 행들을 나타냄
y <- x[complete.cases(x),]    # NA가 포홤된 행들을 제거
head(y)                       # 새로운 데이터셋 y의 내용 확인
```



# 10주차 수업내용
결석 

# 9주차 수업내용

진도 : ???p ~ 

# 7주차 수업내용

## 반복문 다루기

```R
//100번 반복 덧셈
while(1 <= 100){
    sum <- sum + i
    i <- i + 1
}
print(sum)
```

**apply() 계열 함수**

```R
apply(iris[,1:4], 1, mean) # 행 방향으로 함수 적용
apply(iris[,1:4], 2, mean) # 열 방향으로 함수 적용
```

**사용자정의 함수**
```R
함수명 <- function(매개변수 목록) {
    실행할 명령문(들)
    return (함수의 실행 결과)
}
```
```R
// 예시 mydiv라는 인자 2개 받아서 더하는 함수 선언
mydiv <- function(temp1, temp2){
    return temp1 + temp2
}
a <- mydiv(1,2)
a결과값 : 3
```

# 6주차 수업내용

## 매트릭스와 데이터 프레임을 다뤄보자
**매트릭스 데이터프레임에 함수 적용**

숫자로 구성된 매트릭스나 데이터프레임에 대해서 산술연산 적용가능
```R
a <- matrix(1:20, 4, 5)
b <- matrix(21:40, 4, 5)
a <- a*3
b <- b-2

결과값 a
     [,1] [,2] [,3] [,4] [,5]
[1,]    3   15   27   39   51
[2,]    6   18   30   42   54
[3,]    9   21   33   45   57
[4,]   12   24   36   48   60

결과값 b
     [,1] [,2] [,3] [,4] [,5]
[1,]   19   23   27   31   35
[2,]   20   24   28   32   36
[3,]   21   25   29   33   37
[4,]   22   26   30   34   38
```

**매트릭스와 데이터프레임의 자료구조 확인하기**

class(iris) iris 데이터셋의 자료구조 확인

class(state.x77) state.x77 데이터셋의 자료구조 확인

is.matrix(iris) 데이터셋이 매트릭스인지 확인하는 함수

is.data.frame(iris) 데이터셋이 데이터프레임인지 확인하는 함수

**매트릭스와 데이터프레임의 자료구조 변환하기**

----

매트릭스를 데이터프레임으로 변환

st <- data.frame(state.x77)

head(st)

class(st)

------

데이터프레임을 매트릭스로 변환

iris.m <- as.matrix(iris[,1:4])

head(iris.m)

class(iris.m)

**데이터프레임에만 적용되는 열 추출 방법**

iris[, "Species"] 결과가 백터-매트릭스, 데이터프레임 모두 가능

iris[,5]

iris["Species"]

iris[5]

iris$species

**정보 추출**

min(data) 최소값

max(data) 최대값

cat('최소값은', min(data), '이고', '최대값은', max(data),'입니다.') 문자열 더하기

**현재 작업폴더 및 경로 변경**

getwd()

setwd()



# 5주차 수업내용 정리

### 벡터의 연산
- 연산하려는 벡터의 길이가 같아야 함
- 숫자형의 연산은 가능하지만 숫자형 문자형의 연산은 되지 않고 non-numeric 오류를 배출한다.

### 벡터에 사용할 수 있는 함수
- sum(x) : 합계함수
- mix(x) : 최소값 함수
- max(x) : 최대값 함수
- mean(x) : 평균 함수
- median(x) : 중앙값 함수
- range(x) : 범위 함수
- var(x) : 분산 함수
- sd(x) : 표준편차 함수

### 벡터에 논리연산자 적용
```R
d <- c(1,2,3,4,5) #벡터 생성
d>2 #결과값 FALSE, FALSE, TRUE, TRUE, TRUE
d[d>2] #결과값 3,4,5
d[d>2 & d<4] #결과값 3
```
### 논리연산시 Length와 sum의 차이
논리연산자 사용시 반환값은 부울값이니까 length를 해도 조건에 맞는 개수가 나오지 않음
다만 sum을 하게 되면 true는 1이니까 true값만 더해지게 되서 조건에 맞는 개수가 나오게 된다.

### names를 이용해 데이터의 라벨링
```R
> s.day <- s.x + s.y
> names(s.day) <- c("월", "화", "수", "목", "금")
> s.day
월 화 수 목 금 
 3  6  9 12 15 
 ```

### 팩터와 리스트
팩터 : 범주형데이터(자료)를 표현하기 위한 데이터 타입.
원데이터를 가지고 있으면서 중복된 값 제거해서 라벨링
리스트 : 서로 다른 구조의 데이터를 모두 묶은 객체 '(키, 값)' 형태의 데이터를 담는 연관 배열이다.

### 매트릭스
매트릭스, 데이터프레임 : 2차원 자료를 저장하기 위한 대표적인 자료구조

**매트릭스와 데이터프레임의 차이점은 매트릭스에 저장되는 모든 자료의 종류가 동일한 반면 데이터프레임에는 서로 다른 종류의 데이터가 저장가능**

사용방법
```R
> z <- matrix(1:20, nrow=4, ncol=5)
> z
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20
```
기본은 열기준 값 증가
행 기준 값 증가를 하려면 byrow=t 값을 파라미터로 보낸다
```R
> z <- matrix(1:20, nrow=4, ncol=5, byrow=TRUE)
> z
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    6    7    8    9   10
[3,]   11   12   13   14   15
[4,]   16   17   18   19   20
```
열기준 바인딩 cbind(x,y)
행기준 바인딩 rbind(x,y)

### 인덱싱 및 슬라이싱
파이썬에서 하던것이랑 동일
[n:m]n~m행까지 짜르기
[n:m,n2:m2]n~m행 n2~m2열 짜르기
[c(1,2,4),n:m] 백터로 각 행 선택 가능

# 3주차 수업내용 정리
## R 설치하기