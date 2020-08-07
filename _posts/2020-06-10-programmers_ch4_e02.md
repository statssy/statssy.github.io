---
layout: post
title:  "[프로그래머스] 정렬 - 가장 큰 수 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 정렬 - 가장 큰 수 (파이썬)

[코딩테스트 연습 - 정렬 - 가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)를 풀어본다.
  

- 내 코드와 다른 잘 짠 코드랑 로직은 비슷한데 길이차이처럼 실력차이가 난다..
- 결국 숫자를 3자리로 만들어서 솔팅시키는 방법이다.
- 마지막에 str(int)로 바꿈으로써 000을 0으로 바꾼 기술은 정말 배울만 하다.
  
  
### 내 코드

```python
def solution(numbers):
    if max(numbers) == 0:
        answer = '0'
    else:
        numbers_str = list(map(str, numbers))
        max_len = len(str(max(numbers)))
        temp = []
        tup = []
        idx = []
        summ = []
        for numstr in numbers_str:
            if len(numstr) < max_len:
                temp.append(numstr*max_len)
            else :
                temp.append(numstr)

        for n, x in enumerate(temp):
            tup.append((n,x))

        tup.sort(key = lambda element : element[1], reverse=True)

        for t in tup:
            idx.append(t[0])

        for i in idx:
            summ += numbers_str[i]
        answer = ''.join(summ)
    return answer
```
  
  
## 잘 짠 다른 사람 코드
```python
def solution(numbers):
    numbers = list(map(str, numbers))
    numbers.sort(key=lambda x: x*3, reverse=True)
    return str(int(''.join(numbers)))
```
