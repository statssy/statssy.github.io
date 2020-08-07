---
layout: post
title:  "[프로그래머스] 동적계획법 - 타일 장식물 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 동적계획법 - 타일 장식물 (파이썬)

[코딩테스트 연습 - 동적계획법 - 타일 장식물](https://programmers.co.kr/learn/courses/30/lessons/43104)를 풀어본다.
  

- 어제 공부했던 피보나치 수열 문제가 여기서 나오다니 반갑다...동적계획법으로 풀었다..흐뭇..다음 문제는 또 엄청 어렵겠지ㅜ


### 내 코드
```python
def solution(N):
    lst = [0,1]
    for i in range(N+1):
        lst.append(lst[i] + lst[i+1])
    answer = 2*(lst[N] + lst[N+1])
    return answer
```


