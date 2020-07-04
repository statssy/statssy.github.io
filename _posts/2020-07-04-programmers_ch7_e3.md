---
layout: post
title:  "[프로그래머스] 동적계획법 - 정수 삼각형 (파이썬) - Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 코딩테스트 연습 - 동적계획법 - 정수 삼각형 (파이썬)

[코딩테스트 연습 - 동적계획법 - 정수 삼각형](https://programmers.co.kr/learn/courses/30/lessons/43105)를 풀어본다.
  

- 동적계획법 문제인지는 바로 알겠다. 위의 답들이 아래에 영향을 주니까.
- 맨 끝 들은 계산이 한번 되지만 중간에 낀 애들은 위 stair에서 Max값을 가져오면 쉽게(?) 풀수 있다. 참고로 난 쉽지 않았다.
  
  
<img src="https://user-images.githubusercontent.com/28734765/86504845-82125500-bdf8-11ea-852a-fc4265c30783.jpg" width="30%">
  

### 내 코드
```python
def solution(triangle):
    # 공백 2중 리스트 만들기
    lst = []
    for _ in range(len(triangle)):
        lst.append([])
        
    # 인덱스 0은 채워주기
    lst[0].append(triangle[0][0])
    
    # 끝들은 한번씩, 가운데는 위 두개의 맥스
    for stair in range(1, len(triangle)):
        for i in range(0, stair+1): # stair 0 -> 1개
            if i == 0:
                lst[stair].append(lst[stair-1][i] + triangle[stair][i])
            elif i == stair:
                lst[stair].append(lst[stair-1][i-1] + triangle[stair][i])
            else:
                lst[stair].append(max(lst[stair-1][i-1],lst[stair-1][i]) + triangle[stair][i])
    answer = max(lst[len(triangle)-1])
    return answer
```


