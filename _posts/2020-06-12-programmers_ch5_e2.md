---
layout: post
title:  "[프로그래머스] 완전탐색 - 소수 찾기 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 완전탐색 - 소수 찾기 (파이썬)

[코딩테스트 연습 - 완전탐색 - 소수 찾기](https://programmers.co.kr/learn/courses/30/lessons/42839)를 풀어본다.
  

- 소수를 검증 함수와 솔루션 함수를 두개를 만들었고, 속도를 위해 제너레이터를 썼다.
- 잘한 사람의 코드를 보니 set으로 간단하게 해버린게 인상적이다.
  

### 내 코드

```python
import math
from itertools import permutations

def isPrime(num):
    if num == 1: 
        return False

    n = int(math.sqrt(num))
    for k in range(2, n+1):
        if num % k == 0: 
            return False
    return True

def solution(numbers):
    cnt = 0
    prime = set()
    lst = [x for x in numbers]
    for c in range(len(lst)):
        gn = permutations(lst, c+1)
        for g in gn:
            str_join = "".join(g)
            if int(str_join) == 0:
                continue
            elif isPrime(int(str_join)):
                prime.add(int(str_join))
    cnt = len(prime)   
        
    return cnt
```
  
  
### 다른 사람 잘 짠 코드
```python
from itertools import permutations
def solution(n):
    a = set()
    for i in range(len(n)):
        a |= set(map(int, map("".join, permutations(list(n), i + 1))))
    a -= set(range(0, 2))
    for i in range(2, int(max(a) ** 0.5) + 1):
        a -= set(range(i * 2, max(a) + 1, i))
    return len(a)
```
