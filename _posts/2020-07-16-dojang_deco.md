---
layout: post
title:  "[코딩도장] 파이썬 데코레이터 요약본"
subtitle:   "Python"
categories: dev
tags: python
comments: true
---

코딩도장 강의를 보고 파이썬의 데코레이터 요약하려 한다.



### 42.1 데코레이터 만들기
- 메서드를 장식한다. @로 시작하는게 데코레이터
- 사용 이유 : 내가 사용할 함수를 수정하지 않은 상태에서 추가 기능 구현

아래 코드와 같이 함수 시작 끝을 추가하려면 함수를 수정해야 된다.


```python
def hello():
    print('hello 함수 시작')
    print('hello')
    print('hello 함수 끝')
 
def world():
    print('world 함수 시작')
    print('world')
    print('world 함수 끝')
 
hello()
world()
```

    hello 함수 시작
    hello
    hello 함수 끝
    world 함수 시작
    world
    world 함수 끝
    

아래 코드와 같이 함수를 건들지 않고 trace라는 데코레이터를 만든다.  
trace 함수 안에 호출할 함수를 감싸는 wrapper를 만든다.  
wrapper 함수를 다 만들었으면 리턴 해주면 된다.


```python
def trace(func):                             # 호출할 함수를 매개변수로 받음
    def wrapper():                           # 호출할 함수를 감싸는 함수
        print(func.__name__, '함수 시작')    # __name__으로 함수 이름 출력
        func()                               # 매개변수로 받은 함수를 호출
        print(func.__name__, '함수 끝')
    return wrapper                           # wrapper 함수 반환
 
def hello():
    print('hello')
 
def world():
    print('world')
 
trace_hello = trace(hello)    # 데코레이터에 호출할 함수를 넣음
trace_hello()                 # 반환된 함수를 호출
trace_world = trace(world)    # 데코레이터에 호출할 함수를 넣음
trace_world()                 # 반환된 함수를 호출
```

    hello 함수 시작
    hello
    hello 함수 끝
    world 함수 시작
    world
    world 함수 끝
    


```python
위의 코드를 보면 trace 데코레이터에 호출할 함수를 넣어야된다. 그래서 간편하지 못하다.  
그러므로 함수위에 @trace 처럼 넣으므로써 함수 호출을 그대로 살릴 수 있다.
```


```python
def trace(func):                             # 호출할 함수를 매개변수로 받음
    def wrapper():
        print(func.__name__, '함수 시작')    # __name__으로 함수 이름 출력
        func()                               # 매개변수로 받은 함수를 호출
        print(func.__name__, '함수 끝')
    return wrapper                           # wrapper 함수 반환
 
@trace    # @데코레이터
def hello():
    print('hello')
 
@trace    # @데코레이터
def world():
    print('world')
 
hello()    # 함수를 그대로 호출
world()    # 함수를 그대로 호출
```

    hello 함수 시작
    hello
    hello 함수 끝
    world 함수 시작
    world
    world 함수 끝
    

### 42.2 매개변수와 반환값을 처리하는 데코레이션 만들기

지금까지는 매개변수(함수안에 변수들)와 반환값(리턴값)이 없는 함수의 데코레이터 였다.  
아래 코드는 함수의 매개변수화 반환값을 출력하는 데코레이터 이다.
- 참고) *args는 인수들(그냥 숫자), **kwargs는 딕셔너리들(별 두개를 쓴 이유는 하나쓰면 value값이 안나오고 key값이 나옴)


```python
def trace(func):          # 호출할 함수를 매개변수로 받음
    def wrapper(a, b):    # 호출할 함수 add(a, b)의 매개변수와 똑같이 지정
        r = func(a, b)    # func에 매개변수 a, b를 넣어서 호출하고 반환값을 변수에 저장
        print('{0}(a={1}, b={2}) -> {3}'.format(func.__name__, a, b, r))  # 매개변수와 반환값 출력
        return r          # func의 반환값을 반환
    return wrapper        # wrapper 함수 반환
 
@trace              # @데코레이터
def add(a, b):      # 매개변수는 두 개
    return a + b    # 매개변수 두 개를 더해서 반환
 
print(add(10, 20))
```

    add(a=10, b=20) -> 30
    30
    

### 42.2.1 가변 인수 함수 데코레이터
- 위의 ```def add(a,b):```는 매개변수의 개수가 고정된 함수였다. 매개변수가 고정되지않은 함수는 어떻게 처리할까? : 가변 인수 함수로 만들면 됨


```python
def trace(func):                     # 호출할 함수를 매개변수로 받음
    def wrapper(*args, **kwargs):    # 가변 인수 함수로 만듦(위치 인수와 키워드 인수를 모두 받을 수 있도록)
        r = func(*args, **kwargs)    # func에 args, kwargs를 언패킹하여 넣어줌
        print('{0}(args={1}, kwargs={2}) -> {3}'.format(func.__name__, args, kwargs, r))
                                     # 매개변수와 반환값 출력
        return r                     # func의 반환값을 반환
    return wrapper                   # wrapper 함수 반환
 
@trace                   # @데코레이터
def get_max(*args):      # 위치 인수를 사용하는 가변 인수 함수
    return max(args)
 
@trace                   # @데코레이터
def get_min(**kwargs):   # 키워드 인수를 사용하는 가변 인수 함수
    return min(kwargs.values())
 
print(get_max(10, 20))
print(get_min(x=10, y=20, z=30))
```

    get_max(args=(10, 20), kwargs={}) -> 20
    20
    get_min(args=(), kwargs={'x': 10, 'y': 20, 'z': 30}) -> 10
    10
    

### 42.3 매개변수가 있는 데코레이터 만들기
- 데코레이터에 인수가 있을 때 어떻게 할것인가?
- 매개변수가 있는 데코레이터를 만들 때는 함수를 하나 더 만들어야 함.


```python
def is_multiple(x):              # 데코레이터가 사용할 매개변수를 지정
    def real_decorator(func):    # 호출할 함수를 매개변수로 받음
        def wrapper(a, b):       # 호출할 함수의 매개변수와 똑같이 지정
            r = func(a, b)       # func를 호출하고 반환값을 변수에 저장
            if r % x == 0:       # func의 반환값이 x의 배수인지 확인
                print('{0}의 반환값은 {1}의 배수입니다.'.format(func.__name__, x))
            else:
                print('{0}의 반환값은 {1}의 배수가 아닙니다.'.format(func.__name__, x))
            return r             # func의 반환값을 반환
        return wrapper           # wrapper 함수 반환
    return real_decorator        # real_decorator 함수 반환
 
@is_multiple(3)     # @데코레이터(인수)
def add(a, b):
    return a + b
 
print(add(10, 20))
print(add(2, 5))
```

    add의 반환값은 3의 배수입니다.
    30
    add의 반환값은 3의 배수가 아닙니다.
    7
    

### 42.4 클래스로 데코레이터 만들기
- 인스턴스를 호출할수있는 ```__call__``` 메서드를 만들면 된다.


```python
class Trace:
    def __init__(self, func):    # 호출할 함수를 인스턴스의 초깃값으로 받음
        self.func = func         # 호출할 함수를 속성 func에 저장
 
    def __call__(self):
        print(self.func.__name__, '함수 시작')    # __name__으로 함수 이름 출력
        self.func()                               # 속성 func에 저장된 함수를 호출
        print(self.func.__name__, '함수 끝')
 
@Trace    # @데코레이터
def hello():
    print('hello')
 
hello()    # 함수를 그대로 호출
```

    hello 함수 시작
    hello
    hello 함수 끝
    

그 이후것들은 자료들 보면서 하면 되겠다.



참고 : [코딩도장-Python](https://dojang.io/mod/page/view.php?id=2427)


