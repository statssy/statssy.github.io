---
layout: post
title:  "[프로그래머스] 기능개발 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 기능개발 (파이썬)

[코딩테스트 연습 - 기능개발](https://programmers.co.kr/learn/courses/30/lessons/42586)를 풀어본다.
  

- 앞의 수보다 작으면 앞의 수로 바꿔서 리스트 만들고 수 세기
  
```python
def solution(progresses, speeds):
    day = []
    N = len(progresses)
    for i in range(N):
        for b in range(100):
            if progresses[i] + speeds[i] * b >= 100:
                day.append(b)
                break

    for i in range(N-1):
        if day[i] > day[i+1]:
            day[i+1] = day[i]

    cnt = 1
    answer = []
    for i in range(N-1):
        if day[i] == day[i+1]:
            cnt += 1
        else:
            answer.append(cnt)
            cnt = 1
    answer.append(cnt)
    
    return answer
```
  
