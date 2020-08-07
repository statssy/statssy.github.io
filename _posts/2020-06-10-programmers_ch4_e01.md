---
layout: post
title:  "[프로그래머스] 정렬 - K번째수 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 정렬 - K번째수 (파이썬)

[코딩테스트 연습 - 정렬 - K번째수](https://programmers.co.kr/learn/courses/30/lessons/42748)를 풀어본다.
  

- 솔팅의 기본인 어렵지 않은 문제
  

### 내 코드

```python
def solution(array, commands):
    answer = []
    for i in commands:
        temp = array[i[0]-1:i[1]]
        temp.sort()
        answer.append(temp[i[2]-1])   
    return answer
```
