---
layout: post
title:  "[프로그래머스] 탐욕법 - 단속카메라 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 탐욕법 - 단속카메라 (파이썬)

[코딩테스트 연습 - 탐욕법 - 단속카메라](https://programmers.co.kr/learn/courses/30/lessons/42884)를 풀어본다.
  

- 각 이동 경로당 나간 지점(최종점) 순으로 정렬을 시킨다(왜냐면 그래야 순서대로 카메라를 세울 수 있다.) -> 카메라 세운 자리에 다른 경로도 포함하면 체킹해준다. 복잡도 O(N^2)
- 다른사람 잘 짠 코드의 핵심은 last_camera를 만들어서 계속 갱신 해주면서 갯수를 세는 방법이다. 복잡도 O(N)

### 내 코드

```python
def solution(routes):
    routes = sorted(routes, key=lambda x: x[1])
    answer = 0
    check = [0] * len(routes)
    for i in range(0, len(routes)):
        if check[i] == 0:
            camera = routes[i][1]
            answer += 1
        for j in range(i+1, len(routes)):
            if check[j] == 0 and routes[j][1] >= camera >= routes[j][0]:
                check[j] = 1
    return answer
```


### 다른사람 잘 짠 코드
  
```python
def solution(routes):
    routes = sorted(routes, key=lambda x: x[1])
    last_camera = -30000

    answer = 0

    for route in routes:
        if last_camera < route[0]:
            answer += 1
            last_camera = route[1]

    return answer
```
