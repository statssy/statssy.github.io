---
layout: post
title:  "[프로그래머스] 힙(Heap) - 더 맵게 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 힙(Heap) - 더 맵게 (파이썬)

[코딩테스트 연습 - 힙(Heap) - 더 맵게](https://programmers.co.kr/learn/courses/30/lessons/42626)를 풀어본다.
  

- sort로 풀었다가 엄청 느려서 0점 나왔다. heap을 이용했더니 엄청나게 빨랐다.(힙정렬이 시간복잡도 nlog2n이다. 나중에 정렬 알고리즘 시간복잡도 비교해보자)
- 자꾸 16번이 에러가 떴다. 사람들 후기를 보니 [1,2,3], 13 의 경우를 해보래서 보니까 마지막에 1개가 딱 답이 될경우가 체킹이 안되는거 같아서 추가적으로 넣었다.
- while문으로 돌리고 싶은데 먼가 더 속도가 안나오더라 그래서 약간은 반억지로 for문을 사용하였다.
  

### 내 코드

```python
import heapq

def solution(scoville, K):
    heapq.heapify(scoville)
    for cnt in range(len(scoville)):     
        min1 = heapq.heappop(scoville)
        if min1 >= K:
            answer = cnt
            break
        min2 = heapq.heappop(scoville)
        heapq.heappush(scoville, min1 + 2*min2)
        if len(scoville) == 1:
            if scoville[0] >= K:
                answer = cnt+1
                break
            answer = -1
            break

    return answer
```
  
