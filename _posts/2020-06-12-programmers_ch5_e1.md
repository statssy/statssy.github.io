---
layout: post
title:  "[프로그래머스] 완전탐색 - 모의고사 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 완전탐색 - 모의고사 (파이썬)

[코딩테스트 연습 - 완전탐색 - 모의고사](https://programmers.co.kr/learn/courses/30/lessons/42840)를 풀어본다.
  

- 완전 탐색 문제 답게 전체를 다돌렸다. 근데 다른사람의 잘짠 코드 보니까 enumerate와 % 콤보로 내 코드를 압살시켰다. 열심히 공부하자..
  

### 내 코드

```python
def solution(answers):
    people = []
    cnt_1, cnt_2, cnt_3 = 0, 0, 0
    supo_1 = [1, 2, 3, 4, 5]
    supo_2 = [2, 1, 2, 3, 2, 4, 2, 5]
    supo_3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]
    supo_1_lst = supo_1_lst = (supo_1*len(answers))[:len(answers)]
    supo_2_lst = supo_2_lst = (supo_2*len(answers))[:len(answers)]
    supo_3_lst = supo_3_lst = (supo_3*len(answers))[:len(answers)]
    for i in range(len(answers)):
        if answers[i] == supo_1_lst[i]:
            cnt_1 += cnt_1 + 1
        if answers[i] == supo_2_lst[i]:
            cnt_2 += cnt_2 + 1
        if answers[i] == supo_3_lst[i]:
            cnt_3 += cnt_3 + 1
    lst = [cnt_1, cnt_2, cnt_3]
    for i in range(len(lst)):
        if lst[i] == max(lst):
            people.append(i+1)
            
    return people
```
  
  
### 다른 사람 잘 짠 코드
```python
def solution(answers):
    pattern1 = [1,2,3,4,5]
    pattern2 = [2,1,2,3,2,4,2,5]
    pattern3 = [3,3,1,1,2,2,4,4,5,5]
    score = [0, 0, 0]
    result = []

    for idx, answer in enumerate(answers):
        if answer == pattern1[idx%len(pattern1)]:
            score[0] += 1
        if answer == pattern2[idx%len(pattern2)]:
            score[1] += 1
        if answer == pattern3[idx%len(pattern3)]:
            score[2] += 1

    for idx, s in enumerate(score):
        if s == max(score):
            result.append(idx+1)

    return result
```
