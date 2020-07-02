---
layout: post
title:  "[프로그래머스] 동적계획법 - N으로 표현 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 동적계획법 - N으로 표현 (파이썬)

[코딩테스트 연습 - 동적계획법 - N으로 표현](https://programmers.co.kr/learn/courses/30/lessons/42895)를 풀어본다.
  

- 아직은 동적계획법이 어려워서 최대한 고민한 후(4시간?) 다른 사람의 잘 짠 코드를 보고 공부하려한다.

- 첫번째 풀이 하신 분이 정말 잘 정리 한 것 같고, 동적계획법의 메모이제이션을 가장 잘 보여주신 것 같다.

> 만약, N=5일때
> 
> arr[0] -> 5
> 
> arr[1] -> 55 , 5+5, 5-5, 5*5, 5//5
> 
> 이런식으로 전개된다
> 
> 5를 4번 사용한 경우(arr[3])를 생각해보면,
> 
> arr[0] (+ - * //) arr[2]
> 
> arr[1] (+ - * //) arr[1]
> 
> arr[2] (+ - * //) arr[0]

- 두번째 풀이 또한 비슷한 형식으로 풀었음



### 다른 사람 잘 짠 코드1


```python
def solution(N, number):
    arr = [set() for i in range(8)]
    
    # enumerate를 이용해서 i로 찢고 num.add를 이용해서 바로 arr안에 있는 set안에 num을 넣는 형태를 취함
    for i, num in enumerate(arr, start=1):
        num.add(int(str(N) * i))
    
    # 경우의 수를 모두 찾기 위해 for문 다수 씀
    for i in range(1,len(arr)):
        for j in range(i):
            for op1 in arr[j]:
                for op2 in arr[i - j - 1]:
                    arr[i].add(op1 + op2)
                    arr[i].add(op1 - op2)
                    arr[i].add(op1 * op2)
                    if op2 != 0:
                        arr[i].add(op1 // op2)
        if number in arr[i]:
            answer = i+1
            break
    else:
        answer = -1
        
    return answer
```


```python
solution(5, 12)
```

    [{5}, {0, 1, 10, 55, 25}, {555}, {5555}, {55555}, {555555}, {5555555}, {55555555}]
    [{5}, {0, 1, 10, 55, 25}, {0, 2, 4, 5, 6, 555, -20, -4, -50, 15, 11, 50, 275, 20, -5, 60, 125, 30}, {5555}, {55555}, {555555}, {5555555}, {55555555}]





    4

  
[참고 블로그](https://hazung.tistory.com/61)
  
  
### 다른 사람 잘 짠 코드2


```python
def solution(N, number):
    S = [{N}]
    for i in range(2, 9):
        lst = [int(str(N)*i)]     
        for X_i in range(0, int(i / 2)):
            for x in S[X_i]:
                for y in S[i - X_i - 2]:
                    lst.append(x + y)
                    lst.append(x - y)
                    lst.append(y - x)
                    lst.append(x * y)
                    if x != 0: lst.append(y // x)
                    if y != 0: lst.append(x // y)
        if number in set(lst):
            return i
        S.append(lst)
        print('S',S)
    return -1
```


```python
solution(5, 12)
```

    S [{5}, [55, 10, 0, 0, 25, 1, 1]]
    S [{5}, [55, 10, 0, 0, 25, 1, 1], [555, 60, -50, 50, 275, 11, 0, 15, -5, 5, 50, 2, 0, 5, 5, -5, 0, 0, 5, 5, -5, 0, 0, 30, -20, 20, 125, 5, 0, 6, 4, -4, 5, 0, 5, 6, 4, -4, 5, 0, 5]]



    4


