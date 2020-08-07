---
layout: post
title:  "[프로그래머스] 힙(Heap) - 이중우선순위큐 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 힙(Heap) - 이중우선순위큐 (파이썬)

[코딩테스트 연습 - 힙(Heap) - 이중우선순위큐](https://programmers.co.kr/learn/courses/30/lessons/42628)를 풀어본다.
  

- 최대힙이 가장 어려운 문제인데, 효율이 좀 안좋지만 max를 이용했다. 그래도 시간초과는 뜨지 않았다.
  

### 내 코드

```python
import heapq
from collections import deque

def solution(operations):
    answer = []
    q = []
    for op in operations:
        if op[0] == 'I':
            num = int(op[2:])
            heapq.heappush(q, num)
        if op == 'D 1' and q:
            max_tmp = max(q)
            q.pop(q.index(max_tmp))
        if op == 'D -1' and q:
            heapq.heappop(q)

    if q:
        answer.append(max(q))
        answer.append(heapq.heappop(q))
        answer.sort(reverse=True)
    else:
        answer = [0,0]
    return answer
```
  
