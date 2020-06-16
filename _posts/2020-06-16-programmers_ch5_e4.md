---
layout: post
title:  "[프로그래머스] 완전탐색 - 카펫 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 완전탐색 - 카펫 (파이썬)

[코딩테스트 연습 - 완전탐색 - 카펫](https://programmers.co.kr/learn/courses/30/lessons/42842)를 풀어본다.
  

- 세로는 일단 3이상이여야 하므로 3부터 range하고 가능한 경우의 수를 리스트에 넣었다.
- 둘러쌓는게 brown이니까 둘레가 brown과 같으면 answer로 리턴해 주었다.
  

### 내 코드

```python
def solution(brown, yellow):
    whole = brown + yellow
    available_lst = []

    for w in range(3, whole//3+1):
        if whole % w == 0 and w <= whole/w:
            available_lst.append([int(whole/w), w])
            
    for avail in available_lst:
        if sum(avail) * 2 - 4 == brown:
            answer = avail
    return answer
```
