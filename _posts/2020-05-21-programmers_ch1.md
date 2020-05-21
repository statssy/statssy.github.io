---
layout: post
title:  "[프로그래머스] 코딩테스트 연습 - 해시 (파이썬)"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 해시

[코딩테스트 연습 - 해시](https://programmers.co.kr/learn/courses/30/parts/12077)를 풀어본다.
  
파이썬 기초가 부족한것 같아서 다시 시작하는 마음으로 하려 한다.
  
  
### 완주하지 못한 선수
```python
from collections import Counter
def solution(participant, completion):
    answer = ''
    counter = Counter(participant)
    for p in completion:
        counter[p] -= 1
    most = counter.most_common(1)
    answer = most[0][0]
    return answer
```
  
  

### 전화번호 목록
```python
def solution(phone_book):
    for p1 in phone_book:
        for p2 in phone_book:
            if p1 != p2:
                if p1.startswith(p2):
                    return False
    return True
```
  
  

### 위장
```python
from collections import Counter
def solution(clothes):
    answer = 0
    counter = Counter(c[1] for c in clothes)
    temp = 1
    for v in counter.values():
        temp *= (v+1)
    answer = temp - 1

    return answer
```
  
  
  
### 베스트 앨범
: 다른 사람 코드를 이해하는걸로 하자.

```python
from collections import defaultdict
from operator import itemgetter

def solution(genres, plays):
    genre_play_dict = defaultdict(lambda: 0)
    for genre, play in zip(genres, plays):
        genre_play_dict[genre] += play

    genre_rank = [genre for genre, play in sorted(genre_play_dict.items(), key=itemgetter(1), reverse=True)]

    final_dict = defaultdict(lambda: [])
    for i, genre_play_tuple in enumerate(zip(genres, plays)):
        final_dict[genre_play_tuple[0]].append((genre_play_tuple[1], i))

    answer = []
    for genre in genre_rank:
        one_genre_list = sorted(final_dict[genre], key=itemgetter(0), reverse=True)
        if len(one_genre_list) > 1:
            answer.append(one_genre_list[0][1])
            answer.append(one_genre_list[1][1])
        else:
            answer.append(one_genre_list[0][1])

    return answer
```
