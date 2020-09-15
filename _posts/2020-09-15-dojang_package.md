---
layout: post
title:  "[코딩도장] 파이썬 패키지 만들기 요약본"
subtitle:   "Python"
categories: dev
tags: python
comments: true
---

모듈과 패키지 만드는것이 매일 헷갈려서 내 방식대로 정리해 두려고 한다.  
이번 요약은 패키지에 대해서 해본다.

---

# Unit 45. 모듈과 패키지 만들기
복습해 보자. 파이썬 스크립트를 작성할때면 비슷한 클래스와 함수를 작성하게 된다. 그러므로 공통되는 부분은 빼내서 모듈과 패키지로 만들면 된다.

- 모듈(module) : 변수, 함수, 클래스 등을 모아 놓은 스크립트 파일(간단한 기능만 담을때 사용)
- 패키지(package)는 여러 모듈을 묶은 것(코드가 많고 복잡할때 사용. 패키지는 기능들이 모듈로 여러개 나뉘어 있고 관련된 모듈끼리 폴더에 모여 있는 형태)

---

## 45.3 패키지 만들기
(요약) 폴더안에 `__init__.py`가 있으면 패키지다.

![패키지 폴더 구성](https://dojang.io/pluginfile.php/14075/mod_page/content/5/045005.png)  

이렇게 프로젝트 폴더안에 `__init__.py`파일로 저장한다.(파일 내용은 비워 둘 수 있다.)  
폴더(디렉터리) 안에 이 파일이 있으면 해당 폴더는 패키지로 인식된다.(파이썬 3.3부터는 굳이 안넣어도 되지만 하위 버전과 호환시키기 위해 넣자)

### 1. 패키지에 모듈 만들기

> calcpkg/operation.py

```python
def add(a, b):
    return a + b
 
def mul(a, b):
    return a * b
```

> calcpkg/geometry.py

```python
def triangle_area(base, height):
    return base * height / 2
 
def rectangle_area(width, height):
    return width * height
```


### 2-1 패키지 사용하기 방법1 (그림의 main에서 가져올거임. 파일 위치를 잘보자.)

```
import 패키지.모듈
패키지.모듈.변수
패키지.모듈.함수()
패키지.모듈.클래스()
```

> main.py

```python
import calcpkg.operation    # calcpkg 패키지의 operation 모듈을 가져옴
import calcpkg.geometry     # calcpkg 패키지의 geometry 모듈을 가져옴
 
print(calcpkg.operation.add(10, 20))    # operation 모듈의 add 함수 사용
print(calcpkg.operation.mul(10, 20))    # operation 모듈의 mul 함수 사용
 
print(calcpkg.geometry.triangle_area(30, 40))    # geometry 모듈의 triangle_area 함수 사용
print(calcpkg.geometry.rectangle_area(30, 40))   # geometry 모듈의 rectangle_area 함수 사용
```


### 2-2 패키지 사용하기 방법2

> main.py

```python
>>> from calcpkg.operation import add, mul
>>> add(10, 20)
30
>>> mul(10, 20)
200
```

---

## 45.4 패키지에서 from import 응용하기

### 1-1 모듈을 미리 __init__에 넣기. 방법1

- from . import 모듈

> `calcpkg/__init__.py`

```python
from . import operation    # 현재 패키지에서 operation 모듈을 가져옴
from . import geometry     # 현재 패키지에서 geometry 모듈을 가져옴
```

여기서 .(점)은 현재 패키지라는 뜻(지금 init이 패키지 안에 있으니까)  
이렇게 넣어주면 `main.py`에서 쓸때 `import calcpkg`만 쓰면 된다.


### 1-2 모듈을 미리 __init__에 넣기. 방법2

- 현재 패키지니까 .(점)을 붙이고 모듈명과 함수 등 가져오기

    - from .모듈 import 변수, 함수, 클래스

> `calcpkg/__init__.py`

```python
# 현재 패키지의 operation, geometry 모듈에서 각 함수를 가져옴
from .operation import add, mul
from .geometry import triangle_area, rectangle_area
```


### 1-3 모든 함수 가져오기

> `calcpkg/__init__.py`

```python
from .operation import *    # 현재 패키지의 operation 모듈에서 모든 변수, 함수, 클래스를 가져옴
from .geometry import *     # 현재 패키지의 geometry 모듈에서 모든 변수, 함수, 클래스를 가져옴
```


### (참고) `__all__`로 필요한 것만 공개하기

> `calcpkg/__init__.py`

```python
__all__ = ['add', 'triangle_area']    # calcpkg 패키지에서 add, triangle_area 함수만 공개
 
from .operation import *    # 현재 패키지의 operation 모듈에서 모든 변수, 함수, 클래스를 가져옴
from .geometry import *     # 현재 패키지의 geometry 모듈에서 모든 변수, 함수, 클래스를 가져옴
```

<br>
<br>
[참고 : 코딩도장 - 모듈 만들기](https://dojang.io/mod/page/view.php?id=2449)