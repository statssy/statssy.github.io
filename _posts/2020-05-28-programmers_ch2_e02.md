---
layout: post
title:  "[프로그래머스] 다리를 지나는 트럭 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 다리를 지나는 트럭 (파이썬)

[코딩테스트 연습 - 다리를 지나는 트럭](https://programmers.co.kr/learn/courses/30/lessons/42583)를 풀어본다.
  

- queue 에서 pop(0)로 하면 속도가 나오지 않기 때문에 deque를 이용해서 popleft(0)로 뽑아 냈다.
  
```python
from collections import deque

def solution(bridge_length, weight, truck_weights):
    queue = deque([0] * bridge_length)
    sec = 0
    while queue:
        queue.popleft()
        sec += 1
        if truck_weights:
            if sum(queue) + truck_weights[0] <= weight:
                queue.append(truck_weights.pop(0))
            else:
                queue.append(0)    
    return sec
```
  
