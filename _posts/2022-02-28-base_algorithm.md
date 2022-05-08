---
layout: post
title:  "[알고리즘] 완전탐색, 탐욕법, 동적계획법 간단 정리"
subtitle:   "코딩테스트"
categories: alg
tags: prs
comments: true
---

알고리즘은 2년만인가.... 앞에 놓여진 일만 하다보니, 기본이 되는 알고리즘이나 자료구조쪽에 대해 까먹은 것도 있고 해서 그냥 썼다.

---

1. 완전탐색(Brute Force)
- 모든 선택지 탐색하는 방법.
- 순서
1) 해결하고자 하는 문제의 가능한 경우의 수를 대략적으로 계산한다.(10만인데 O(n^2) 복잡도라면?)
2) 가능한 모든 방법을 다 고려한다.
3) 실제 답을 구할 수 있는지 적용한다.
  
여기서 2)의 모든 방법에는 다음과 같은 방법 등이 있다.
  
① Brute Force 기법 - 반복 / 조건문을 활용해 모두 테스트하는 방법
② 순열(Permutation) - n개의 원소 중 r개의 원소를 중복 허용 없이 나열하는 방법
③ 재귀 호출
④ 비트마스크 - 2진수 표현 기법을 활용하는 방법
⑤ BFS, DFS를 활용하는 방법
  
  
2. 탐욕법(Greedy)
- 한 문제를 작은 문제로 쪼개서 재귀로 푸는 것은 완전탐색, 동적계획법과 동일하나, 각 단계마다 당장 가장 좋은 방법만 선택(ex. 최소 동전으로 거슬러 주기)
① 여러 부분 문제로 나누기
② 어떤 우선 순위로 선택할지 결정 (손으로 몇 개 풀면서)
③ 답이 항상 최적해인지, 각 단계에서 최적해가 전체 최적해를 만드는지 생각
  
  
3. 동적계획법(Dynamic Programming)
- 완전 탐색처럼 해결하나, 중복되는 sub 문제를 한번만 계산되도록 memoization을 활용하는 문제(ex. 피보나치)
- 사용조건
① Memoization(하향식 접근 Top-Down) : 중복 계산 한번만 메모, 재귀함수 사용
② Tabulation(상향식 접근 Bottom-Up) : 반복문을 기반으로 코드 작성.


--- 
# 참고
https://noteforstudy.tistory.com/17
https://m.blog.naver.com/jss5187/221926895677
https://hongjw1938.tistory.com/78