---
layout: post
title:  "[프로그래머스] 탐욕법 - 큰 수 만들기 (파이썬) - Statssy"
subtitle:   "프로그래머스"
categories: alg
tags: prs
comments: true
---

## 코딩테스트 연습 - 탐욕법 - 큰 수 만들기 (파이썬)

[코딩테스트 연습 - 탐욕법 - 큰 수 만들기](https://programmers.co.kr/learn/courses/30/lessons/42883)를 풀어본다.
  

- 내 코드는 일단 시간초과가 3개가 나온다. 왜냐하면 number가 1,000,000자리인데 O(n^2) 복잡도가 나오기 때문인거 같다.
- 잘 푼 사람을 보니 역시 pop으로 풀었다. stack에 채우다가 큰수가 나오면 이전 stack을 지우는 형식이다.
- 최대한 stack을 이용한 코드를 짜야 한다. 오늘도 열심히

### 내 코드

```python
def solution(number, k):
    ans = []
    number_list = [i for i in number]
    n = len(number_list)
    i = 0
    while True:
        temp = number_list[:-n+k+i+1] if (-n+k+i+1) < 0 else number_list
        for j in range(len(temp)):
            if temp[j] == max(temp):
                ans.append(temp[j])
                number_list = number_list[j+1:]
                i += 1
                break
        if len(ans) == n - k:
            break
    answer = ''.join(ans)

    return answer
```

### 잘 푼 다른 사람 코드

```python
def solution(number, k):
    stack = [number[0]]
    for num in number[1:]:
        while len(stack) > 0 and stack[-1] < num and k > 0:
            k -= 1
            stack.pop()
        stack.append(num)
    if k != 0:
        stack = stack[:-k]
    return ''.join(stack)
```
