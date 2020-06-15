---
layout: post
title:  "[프로그래머스] 완전탐색 - 숫자 야구 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 완전탐색 - 숫자 야구 (파이썬)

[코딩테스트 연습 - 완전탐색 - 숫자 야구](https://programmers.co.kr/learn/courses/30/lessons/42841)를 풀어본다.
  

- 이 비슷한 문제를 2년전?에 푼거 같은데 이젠 내 능력으로 풀 수 있더라..뿌듯
- for문이 너무 많아서 약간 지저분해 보이긴하는데 핵심은 그냥 strike는 같은자리 구하면되고 ball은 같은 수가 몇개 있는지 보고 그 수에 strike수를 뺀 값으로 했다.
  

### 내 코드

```python
from itertools import permutations
def solution(baseball):
    perm = permutations([i for i in range(1,10)], 3)
    sum = 0
    for p in perm:
        ans = list(p)
        cnt = 0
        for b in baseball:
            strike = 0 
            ball= 0
            test = list(map(int, (str(b[0]))))
            for i in range(3):
                if ans[i] == test[i]:
                    strike += 1
            for t in test:
                if t in ans:
                    ball += 1
            ball = ball - strike
            if b[1] == strike and b[2] == ball:
                cnt += 1
        if cnt == len(baseball):
            sum += 1
    return sum
```
