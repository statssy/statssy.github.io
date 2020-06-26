---
layout: post
title:  "[프로그래머스] 구명보트 - 조이스틱 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 구명보트 - 조이스틱 (파이썬)

[코딩테스트 연습 - 구명보트 - 조이스틱](https://programmers.co.kr/learn/courses/30/lessons/42885)를 풀어본다.
  

- 처음부터 sort를 써서 효율성 측면에서 비효율적이라고 생각했는데 정확성/효율성에서 만점을 받았다.
- 큰수부터 뺴면서 혹시 작은수도 같이 뺄수 있으면 빼는 코드다.

### 내 코드

```python
from collections import deque

def solution(people, limit):
    people.sort()
    queue = deque(people)
    cnt = 0
    while queue:
        b = queue.pop()
        if not queue:
            cnt += 1
            break
        elif b + queue[0] <= limit:
            queue.popleft()
        cnt += 1
    return cnt
```
