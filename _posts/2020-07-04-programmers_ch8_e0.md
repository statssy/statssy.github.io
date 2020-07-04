---
layout: post
title:  "[DFS와 BFS] 기본 문제를 풀며 고찰 (파이썬)- Statssy"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

## 공부하기 전에
프로그래머스에서 이제 DFS와 BFS 문제까지 왔다. 사실 한 6개월 전에 백준 문제에서 푼 적이 있지만, 부끄럽지만 사실 그때는 이해를 제대로 못한채 풀었던 것 같다.  
지금도 충분히 이해했다고 보진 않지만, 또 6개월 뒤에 내가 포스팅을 하면서 지금의 나를 반성하고 있을지는 모르겠다.  
여튼 다이나믹 프로그래밍(=동적 계획법) 문제를 풀면서 하위문제를 쪼개서 상위문제를 풀어야하는데, 경우의수들만 연구하다가 시간을 지체하는 경우가 많았고, 내가 생각치 못한 예외를 골라내기 쉽지 않았다.  
그래서 팁을 보니까 다들 DFS BFS로 풀었다고 하더라. 아마 약간 만능 개념인가? 여튼 백문이불여일견이라 했다. 일단 학습해 보자.


## DFS와 BFS에 관하여  
- DFS : 깊이 우선 탐색(DEPTH FISRT SEARCH) 말그대로 DEPTH(깊이)로 오지게 파고 들어가는 형식이다. 그럴려면 스택 형식으로 최근에 넣은거 빼야 되므로 pop()을 쓰면 됨. 시간 복잡도는 O(V+N) 노드수+간선수
- BFS : 너비 우선 탐색(BREADTH FIRST SEARCH) 이거 또한 말그대로 BREADTH(너비=폭) 같은 폭에 있는(약간 같은 층 개념)에 있는 애들부터 뺴는 형식으로 큐를 쓴다. pop(0) 쓰면 됨 시간복잡도 DFS랑 같음


- 참고 사이트(이거 그림 보고 공부하면 아주 이해 잘됨)
[깊이 우선 탐색(DFS)](https://www.fun-coding.org/Chapter18-dfs-live.html)
[너비 우선 탐색(BFS)](https://www.fun-coding.org/Chapter18-bfs-live.html)

### 데이터 


```python
graph = dict()

graph['A'] = ['B', 'C']
graph['B'] = ['A', 'D']
graph['C'] = ['A', 'G', 'H', 'I']
graph['D'] = ['B', 'E', 'F']
graph['E'] = ['D']
graph['F'] = ['D']
graph['G'] = ['C']
graph['H'] = ['C']
graph['I'] = ['C', 'J']
graph['J'] = ['I']
```

### DFS 알고리즘 구현
- DFS 시간 복잡도 : O(V+E), 노드수 간선수의 합
- 큐 1개, 스택 1개


```python
def dfs(graph, start_node):
    visited, need_visit = list(), list()
    need_visit.append(start_node)
    
    while need_visit:
        node = need_visit.pop() # 맨 뒤에꺼 빼기
        if node not in visited:
            visited.append(node)
            need_visit.extend(graph[node]) # append와 extend 차이 알기
    
    return visited
```


```python
dfs(graph, 'A')
```




    ['A', 'C', 'I', 'J', 'H', 'G', 'B', 'D', 'F', 'E']



### BFS 알고리즘 구현
- BFS 시간 복잡도 : O(V+E)
- 큐 2개


```python
def bfs(graph, start_node):
    visited = list()
    need_visit = list()
    
    need_visit.append(start_node)
    
    while need_visit:
        node = need_visit.pop(0) # 맨 앞에꺼 빼기
        if node not in visited:
            visited.append(node)
            need_visit.extend(graph[node])
    
    return visited
```


```python
bfs(graph, 'A')
```




    ['A', 'B', 'C', 'D', 'G', 'H', 'I', 'E', 'F', 'J']


