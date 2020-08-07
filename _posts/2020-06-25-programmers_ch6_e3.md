---
layout: post
title:  "[프로그래머스] 탐욕법 - 조이스틱 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 탐욕법 - 조이스틱 (파이썬)

[코딩테스트 연습 - 탐욕법 - 조이스틱](https://programmers.co.kr/learn/courses/30/lessons/42860)를 풀어본다.
  

- 내 코드는 일단 테스트 11이 실패한다. 실패와 상관없이 내 코드는 틀렸다. 왜냐하면 'BBABAAAB' 경우를 생각하지 못했기 때문이다. 그래서 이문제는 사실 그리디 문제가 아닌 BFS문제인거 같다.
- 질문하기에 다른분이 짜놓은 코드가 있어서 보려했지만 FLOW는 알겠지만 아직은 감이 안온다. BFS, DFS문제를 좀 풀어보고 이문제를 다시 보려 한다.
- 내 코드의 핵심은 오른쪽으로 쭉가는게 이득인지 왼쪽으로 쭉가는게 이득인지, 그리고 마지막에 A가 다수 나오면 그 만큼은 계산하지 않는다 라는 코드로 짰지만 다시 말하지만 'BBABAAAB' 경우를 고려하지 않은 좋지않은 수였다.

### 내 코드

```python
def locate(name, ord_lst):
    loc = 0
    right = 0
    left = 0
    cnt = 0
    for i in range(1, len(name)):
        if ord_lst[i] == 0:
            right += 1
        else:
            break
    for i in range(1, len(name)):
        if ord_lst[-i] == 0:
            left += 1
        else:
            break
            
    if right >= left:
        loc = -1
        cnt = right
    else:
        loc = 1
        cnt = left
    return loc, cnt


def solution(name):
    answer = 0
    ord_lst = []
    for s in [str(i) for i in name]:
        if (ord(s)-65) <= 13:
            ord_lst.append(ord(s)-65)
        else:
            ord_lst.append(-(ord(s)-65-26))
    
    loc, cnt = locate(name, ord_lst)
    answer = sum(ord_lst)+(len(ord_lst)-1)-cnt
    return answer
```

### 잘 푼 다른 사람 코드

```python
import copy
def solution(name):
    moves = [-1, 1]
    nameList = list(name)
    # start = (nameList, index, count)
    def bfs(start):
        stack = [start]
        
        while stack:
            n = stack.pop()
            array, idx, cnt = n[0], n[1], n[2]
            cnt += min(ord(array[idx]) - 65, 91 - ord(array[idx])) # 아스키 코드값으로 변경 횟수 계산
            array[idx] = 'A'
            if array.count('A') == len(array): # 종료 시점
                return cnt
            for move in moves:
                new_array = copy.deepcopy(array) # array 주소 복사 서로 영향 안미치게
                new_idx = idx + move
                new_cnt = cnt + 1
                stack.insert(0, (new_array, new_idx, new_cnt))

    return bfs((nameList, 0, 0))
```
