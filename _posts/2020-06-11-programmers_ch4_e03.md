---
layout: post
title:  "[프로그래머스] 정렬 - H-Index (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 정렬 - H-Index (파이썬)

[코딩테스트 연습 - 정렬 - H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747)를 풀어본다.
  

- 문제 이해가 어려운 문제, 마지막에 다 만족할 경우에는 논문 n편 모두를 내줘야함
  

### 내 코드

```python
def solution(citations):
    citations.sort(reverse=True)
    for i in range(len(citations)):
        if citations[i] <= i: 
            answer = i
            break
        else:
            answer = len(citations)
    return answer

```
