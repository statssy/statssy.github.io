---
layout: post
title:  "[프로그래머스] 탐욕법 - 저울 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 탐욕법 - 저울 (파이썬)

[코딩테스트 연습 - 탐욕법 - 저울](https://programmers.co.kr/learn/courses/30/lessons/42886)를 풀어본다.
  

- 예를 들어 [1,1,4] 이 있다면 답은 3이다. 왜냐면 1+1 다음에 3이 나와야 숫자가 이어지는데 못 이어버린다. 그래서 내가 만든 아이디어는 1+1 < 4 인 상태면 1+1에서 멈추고 1을 더해주는 형태를 취했다. 
- 만약에 다 해도 안나오면 그냥 전체 값 더하기 1을 해주자
- 다른 사람 잘 짠 코드는 내가 했던 짓들 무참히 밟아버렸다. 처음 답을 1로 시작하면 될 것을...... 슬프다.

### 내 코드
  
```python
from collections import deque
def solution(weight):
    weight.sort()
    queue = deque(weight)
    answer = 0
    total = 0
    prev = 1000000000
    while queue:
        q = queue.popleft()
        total += q
        if prev < q-1:
            answer = prev+1
            break
        prev = total
        answer = total+1
    
    return answer
```

### 다른 사람 잘 짠 코드
  
```python
def solution(weight):
    weight.sort()
    ans = 1
    for e in weight:
        if ans < e:
            break
        ans += e

    return ans
```
