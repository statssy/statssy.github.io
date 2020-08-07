---
layout: post
title:  "[프로그래머스] 힙(Heap) - 디스크 컨트롤러 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 힙(Heap) - 디스크 컨트롤러 (파이썬)

[코딩테스트 연습 - 힙(Heap) - 디스크 컨트롤러](https://programmers.co.kr/learn/courses/30/lessons/42627)를 풀어본다.
  

- 진짜 너무 어려운 코드인것 같다. 가장 나의 풀이랑 비슷한 분의 코드를 가져와 봤다. 좀더 연구할 필요가 있겠다.
- 원초적인 접근을 했다. tasks에서 이어지게 하는 조건이 만족하면 q에 담고, 그것이 아니면 가장 빠른걸 q에 담는다.
- 그러므로 2중 리스트에서도 인덱스 0에서 정렬도 필요하고 인덱스 1에서도 정렬이 필요한 문제였다.
  

### 내 코드

```python
import heapq
from collections import deque

def solution(jobs):
    tasks = deque(sorted([(x[1], x[0]) for x in jobs], key=lambda x: (x[1], x[0])))
    q = []
    heapq.heappush(q, tasks.popleft()) # 태스크에서 순서 제일 빠른 애 q에 넣기
    current_time, total_response_time = 0, 0 # 현재 시간, 전체 시간(누적)
    
    while len(q) > 0: 
        dur, start = heapq.heappop(q) # (3,0) -> (6,2) -> (9,1)
        current_time = max(current_time + dur, start + dur)
        total_response_time += current_time - start # 고유 시작 위치 빼기
        
        # 조건에 맞는 애들 q에다 넣고
        while len(tasks) > 0 and tasks[0][1] <= current_time:
            heapq.heappush(q, tasks.popleft())
            
        if len(tasks) > 0 and len(q) == 0: 
            # 위 조건이 해당 안되서 아무것도 못담았다면
            # 가장 빠른 아이를 q에 넣는다.
            heapq.heappush(q, tasks.popleft())
            
    return total_response_time // len(jobs)

```
  
