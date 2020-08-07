---
layout: post
title:  "[프로그래머스] 프린터 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 프린터 (파이썬)

[코딩테스트 연습 - 프린터](https://programmers.co.kr/learn/courses/30/lessons/42587)를 풀어본다.
  

- deque로 좀 빠르게 만들고 인덱스를 따로 파는 방법으로 했음
- 다른 사람이 더 잘 푼거 같음 ```[()]``` 방식으로 풀면 좀 쉽게 풀릴만한 문제였음
  

### 내 코드

```python
import collections

def solution(priorities, location):
    deq = collections.deque(priorities)
    idx = collections.deque([i for i in range(len(deq))])
    lst = []
    while deq:
        if deq[0] == max(deq):
            deq.popleft()
            lst.append(idx.popleft())
        else:
            deq.append(deq.popleft())     
            idx.append(idx.popleft())
    answer = lst.index(location)+1
    return answer
```
  
  
### 잘푼 사람 코드

```python
def solution(priorities, location):
    queue =  [(i,p) for i,p in enumerate(priorities)]
    answer = 0
    while True:
        cur = queue.pop(0)
        if any(cur[1] < q[1] for q in queue):
            queue.append(cur)
        else:
            answer += 1
            if cur[0] == location:
                return answer
```
  
