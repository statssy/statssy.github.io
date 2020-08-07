---
layout: post
title:  "[프로그래머스] 탑 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 탑 (파이썬)

[코딩테스트 연습 - 탑](https://programmers.co.kr/learn/courses/30/lessons/42588)를 풀어본다.
  

- 뒤로 돌려서 하나씩 돌아가게
  
```python
def solution(heights):
    answer = []
    for i in reversed(range(len(heights))):
        is_True = 1
        for j in reversed(range(len(heights))):
            if heights[i] < heights[j] and i > j :
                answer.append(j+1)
                is_True = 0
                break
        if is_True:
            answer.append(0)
    answer = answer[::-1]
    return answer
```
  
