---
layout: post
title:  "[프로그래머스] 힙(Heap) - 라면공장 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 힙(Heap) - 라면공장 (파이썬)

[코딩테스트 연습 - 힙(Heap) - 라면공장](https://programmers.co.kr/learn/courses/30/lessons/42629)를 풀어본다.
  

- 아직은 이런 코드 짜는게 쉽지 않겠다. 핵심은 공급 필요할 때, 큐에 넣어놨다가 가장 큰거를 stock에 더하는 형태
- 문제의 이해가 부족하면 정말 어렵게 풀어야 할것같다. 이 아래 코드도 다른 분이 푼거를 참고했다.
  

### 내 코드

```python
import heapq
def solution(stock, dates, supplies, k):   
    answer = 0
    idx = 0
    pq = []
    
    while stock < k:
        for i in range(idx, len(dates)):
            if stock < dates[i]:
                break
            heapq.heappush(pq, -supplies[i])     
            idx = i + 1

        stock += (heapq.heappop(pq) * -1)
        answer += 1

    return answer
```
  
