---
layout: post
title:  "[프로그래머스] 탐욕법 - 섬 연결하기 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 탐욕법 - 섬 연결하기 (파이썬)

[코딩테스트 연습 - 탐욕법 - 섬 연결하기](https://programmers.co.kr/learn/courses/30/lessons/42861)를 풀어본다.
  

- 다른 분의 좋은 코드가 있어서 공부해 보려한다. 특이 했던건 connect 리스트를 만들어서 지나간 노드를 저장하는 방식과 최소 가중치와 인덱스를 한번에 가져올수 있게 temp를 만든 방법이 인상적이다.


### 다른사람 잘 짠 코드
  
```python
def solution(n, costs):
    costs.sort()
    connect = [costs[0][0]]
    answer = 0
    while len(connect) != n:
        temp = 10000000000000000
        idx = 0
        for i in range(len(costs)):
            if costs[i][0] in connect or costs[i][1] in connect:
                # 어떤 방향으로든 다 채워진 idx라면 뛰어넘기
                if costs[i][0] in connect and costs[i][1] in connect:
                    continue
                # 가중치와 idx 모두 가져올 수 있게
                if temp > costs[i][2]:
                    temp = costs[i][2]
                    idx = i
        answer += temp
        connect.append(costs[idx][0])
        connect.append(costs[idx][1])
        connect = list(set(connect))
        costs.pop(idx)
    
    return answer
```
[참고 블로그](https://codedrive.tistory.com/164)
