
---
layout: post
title:  "[Python_알고리즘공부] 파이썬 입력함수(input)"
subtitle:   "CodingTest"
categories: pro
tags: coding
comments: true
---

# [Python_알고리즘공부] 파이썬 입력함수(input)


```python
1.(1) map으로 입력 간단하게 만들기
```

[참고 : 파이썬 입력함수(input)](https://dpdpwl.tistory.com/52) 에서 참고 하였다.

파이썬 입력은 input함수를 사용해서 입력할 수 있는데 int형을 입력해도 문자로 인식한다.
그래서 int형으로 만들어 줘야한다.


```python
a = int(input())
```

    5
    


```python
b = int(input())
```

    2
    


```python
print(a*b)
```

    10
    

위와 같은 입력을 간단히 map을 이용하면 된다.


```python
a,b = map(int, input().split())
```

    5 2
    


```python
print(a)
```

    5
    


```python
print(b)
```

    2
    


```python
print(a*b)
```

    10
    

### 1.(2) (추가) 리스트에 map 사용하기

[참고 : 코딩도장(리스트에 map 사용하기)](https://dojang.io/mod/page/view.php?id=2286)

ex1) for문을 이용한 실수가 저장되어 있는 리스트를 정수로 변환


```python
a = [1.2, 2.5, 3.7, 4.6]
```


```python
for i in range(len(a)):
    a[i] = int(a[i])
```


```python
a
```




    [1, 2, 3, 4]



ex2) map을 이용한 리스트 정수로 변환


```python
b = [1.2, 2.5, 3.7, 4.6]
```


```python
b = list(map(int, b))
```


```python
b
```




    [1, 2, 3, 4]



ex3) 리스트 뿐만아니라 모든 반복 가능한 객체도 너을 수 있음


```python
a = list(map(str, range(10)))
```


```python
a
```




    ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']



### 1.(3) (추가) input().split()과 map


```python
input().split()의 결과는 문자열 리스트
```


```python
a = input().split()
```

    10 20
    


```python
a
```




    ['10', '20']




```python
a = map(int, input().split())
```

    10 20
    


```python
a # 맵 객체가 만들어서 져서 안을 볼 수가 없다.
```




    <map at 0x5809160>




```python
list(a)
```




    [10, 20]


