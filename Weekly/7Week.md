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

