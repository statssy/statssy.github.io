---
layout: post
title:  "[프로그래머스] 탐욕법 - 체육복 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 탐욕법 - 체육복 (파이썬)

[코딩테스트 연습 - 탐욕법 - 체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)를 풀어본다.
  

- 앞쪽부터 가능한대로 채우고 그 다음 뒤쪽 가능한대로 채움. 왜냐면 맨앞부터 채워야 왼쪽으로 갈 경우를 제외할 수 있기 때문
- 그 다음 경우는 뒷쪽부터 채우고 그다음 앞쪽 가능한대로 채우는 방법.
- 다른사람 코드는 엄청 간결해서 너무 좋았다. 근데 사람들 댓글이 O(n^2)이라 안좋다고는 하는데, 복잡도가 높은것은 그렇다치고 저렇게 간결하게 한번 짜보고싶다..  

### 내 코드

```python
def solution(n, lost, reserve):
    now = [1] * (n+1)
    for l in lost:
        now[l] -= 1
    for r in reserve:
        now[r] += 1

    for i in range(1,n):
        if now[i] == 2 and now[i+1] == 0:
            now[i], now[i+1] = 1, 1

    for i in range(n,1,-1):
        if now[i] == 2 and now[i-1] == 0:
            now[i], now[i-1] = 1, 1

    summ1 = sum([1 for i in now if i > 0])-1
    
    now = [1] * (n+1)
    for l in lost:
        now[l] -= 1
    for r in reserve:
        now[r] += 1

    for i in range(n,1,-1):
        if now[i] == 2 and now[i-1] == 0:
            now[i], now[i-1] = 1, 1
            
    for i in range(1,n):
        if now[i] == 2 and now[i+1] == 0:
            now[i], now[i+1] = 1, 1
    
    summ2 = sum([1 for i in now if i > 0])-1
    
    answer = max(summ1, summ2)
    
    
    return answer
```

### 잘 푼 다른 사람 코드

```python
def solution(n, lost, reserve):
    _reserve = [r for r in reserve if r not in lost]
    _lost = [l for l in lost if l not in reserve]
    for r in _reserve:
        f = r - 1
        b = r + 1
        if f in _lost:
            _lost.remove(f)
        elif b in _lost:
            _lost.remove(b)
    return n - len(_lost)
```
