---
layout: post
title:  "[동적 프로그래밍] 피보나치 수열 문제 이론 (파이썬)- Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 동적 프로그래밍에 관하여
프로그래머스 그리디 문제를 다 풀고 이번에는 동적 프로그래밍이라는 것이 나왔다. 일단 대충 어떤 느낌인지 써보겠다.  
  
- 탐욕법 : 순간순간 최적의 길찾기
- 동적 프로그래밍 : 전체 문제를 하위 문제로 나누고 하위 문제를 결합하여 최종 문제 해결. 중요한건 메모이제이션! 답을 기억해 놓아서 또 다시 풀지않게 하는것
  
그렇다면 이번에는 피보나치 수열 문제를 처음에는 재귀로 풀어보고 나중에는 동적 프로그래밍으로 풀어보겠다.

### 재귀로 피보나치 수열 풀기


```python
def fibonacci(a,b):
    print(a)
    return fibonacci(b,a+b)
```

### 피보나치 수열에서 재귀를 멈추게끔 세팅하기


```python
def fibonacci(a,b,c):
    print(a)
    if c == 0:
        return a
    return fibonacci(b, a+b, c-1)
```


```python
fibonacci(0,1,10)
```

    0
    1
    1
    2
    3
    5
    8
    13
    21
    34
    55





    55



### 효율성이 떨어지는 모델 (재귀 + 함수 2개)
- 계산이 엄청 나게 많이 됨


```python
def fibo(n):
    # print(n)
    # 이 프린트 문을 이용하면 얼마나 많은 계산을 하는 지 알 수 있다.
    if n < 2:
        return n
    else:
        return fibo(n-1) + fibo(n-2)

print(fibo(10)) # 55
```

    55


### 피보나치 수열 동적 프로그래밍으로 풀기
- 간단한 하위 문제를 해결하면서 좀 더 복잡한 상위 문제로 나아감
- 하위 문제를 해결한 결과는 저장
- 전체 문제를 해결할 때 까지 반복


```python
def dyn_fibo(n):
    val = [0,1]
    if n < 2:
        return val[n]
    else:
        for i in range(2, n+1):
            val.append(val[i-1] + val[i-2])
        return val[n]

print(dyn_fibo(10)) # 55
```

    55


### 동적 프로그래밍 + 슬라이딩 윈도 기법 (v0, v1, v2 돌려막기)


```python
def fib2(n):
    if n < 2:
        return n
    v0, v1 = 0, 1
    for _ in range(n-1):
        v2 = v1 + v0
    return v2

for i in range(11):
    print(fib2(i))
```

    0
    1
    1
    2
    3
    5
    8
    13
    21
    34
    55

[참고 사이트](https://comdoc.tistory.com/entry/33-%ED%94%BC%EB%B3%B4%EB%82%98%EC%B9%98-%EC%88%98%EC%97%B4%EA%B3%BC-%EB%8F%99%EC%A0%81-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)를 참고하였다.
  


