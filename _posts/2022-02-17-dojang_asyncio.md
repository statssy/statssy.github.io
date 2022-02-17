---
layout: post
title:  "[코딩도장] asyncio 사용하기 요약본"
subtitle:   "Python"
categories: dev
tags: python
comments: true
---

내 사수가 떠난다. 슬프다. 이젠 스스로 학습 놀이를 시작해야한다. 나에게 집중력따윈 없으므로 쓰면서 공부한다.

---

### Unit 47.10 asyncio 사용하기
- asyncio(Asynchronous I/O)는 비동기 프로그래밍을 위한 모듈이다. CPU작업과 I/O를 병렬로 처리한다.
- 비동기처리는 여러작업을 처리하도록 예약한 뒤 작업이 끝나면 결과를 받는 방식

### 47.10.1 네이티브 코루틴 만들기
- ```def``` 대신 ```async def``` 로 만들어야한다.
- 이런걸 네이티브 코루틴이라고하는데, 제너레이터 기반의 코루틴과 구분하기 위함이다. (참고로 제너레이터는 yield써서 만드는 그....range같은거!)

```python
import asyncio
 
async def hello():    # async def로 네이티브 코루틴을 만듦
    print('Hello, world!')
 
loop = asyncio.get_event_loop()     # 이벤트 루프를 얻음
loop.run_until_complete(hello())    # hello가 끝날 때까지 기다림
loop.close()
```

- ```asyncio.get_event_loop()```는 이벤트 루프를 얻고(그냥 써야된다고 생각하자)
- ```run.until_complete```는 코루틴 객체 넣어서 끝날때까지 기다림
- 다시말해 루프만들고 그냥 complete될때까지 기다림 그 안의 객체는 당연히 코루틴 객체여야하고!! 그리고 close하면된다.

### 47.10.2 await로 네이티브 코루틴 실행하기
- await는 단어그대로 await 뒤에 객체가 끝날때까지 기다린다.

```python
import asyncio
 
async def add(a, b):
    print('add: {0} + {1}'.format(a, b))
    await asyncio.sleep(1.0)    # 1초 대기. asyncio.sleep도 네이티브 코루틴
    return a + b    # 두 수를 더한 결과 반환
 
async def print_add(a, b):
    result = await add(a, b)    # await로 다른 네이티브 코루틴 실행하고 반환값을 변수에 저장
    print('print_add: {0} + {1} = {2}'.format(a, b, result))
 
loop = asyncio.get_event_loop()             # 이벤트 루프를 얻음
loop.run_until_complete(print_add(1, 2))    # print_add가 끝날 때까지 이벤트 루프를 실행
loop.close()                                # 이벤트 루프를 닫음
```

- 다필요없고 그냥 눈치껏 await는 네이티브 코루틴 안에서만 사용가능하고 다른 네이티브 코루틴 접근할때 써야한다고 생각하면되겠다.


### 47.10.3 비동기로 웹 페이지 가져오기

- 만약에 아래와 같은 코드로 실행하면 오지게 시간이 많이 걸린다.

```python
from time import time
from urllib.request import Request, urlopen
 
urls = ['https://www.google.co.kr/search?q=' + i
        for i in ['apple', 'pear', 'grape', 'pineapple', 'orange', 'strawberry']]
 
begin = time()
result = []
for url in urls:
    request = Request(url, headers={'User-Agent': 'Mozilla/5.0'})    # UA가 없으면 403 에러 발생
    response = urlopen(request)
    page = response.read()
    result.append(len(page))
 
print(result)
end = time()
print('실행 시간: {0:.3f}초'.format(end - begin))
```
- 근데 asyncio로 비동기로 실행하면 빠르다 엄청.

```python
from time import time
from urllib.request import Request, urlopen
import asyncio
 
urls = ['https://www.google.co.kr/search?q=' + i
        for i in ['apple', 'pear', 'grape', 'pineapple', 'orange', 'strawberry']]
 
async def fetch(url):
    request = Request(url, headers={'User-Agent': 'Mozilla/5.0'})    # UA가 없으면 403 에러 발생
    response = await loop.run_in_executor(None, urlopen, request)    # run_in_executor 사용
    page = await loop.run_in_executor(None, response.read)           # run in executor 사용
    return len(page)
 
async def main():
    futures = [asyncio.ensure_future(fetch(url)) for url in urls]
                                                           # 태스크(퓨처) 객체를 리스트로 만듦
    result = await asyncio.gather(*futures)                # 결과를 한꺼번에 가져옴
    print(result)
 
begin = time()
loop = asyncio.get_event_loop()          # 이벤트 루프를 얻음
loop.run_until_complete(main())          # main이 끝날 때까지 기다림
loop.close()                             # 이벤트 루프를 닫음
end = time()
print('실행 시간: {0:.3f}초'.format(end - begin))
```

- 여기서 중요한거는 urlopen이나 response.read 같은 함수는 결과 나올때까지 중단되는데(이런걸 블로킹 I/O라고함), 이럴때는 ```run_in_executor``` 함수를 사용해서 병렬로 실행해야한다.

- ```이벤트루프.run_in_executor(None, 함수, 인수1, 인수2, 인수3)``` 문법은 그냥 이걸 기억하면된다. 인수가 길면 쭉쭉 쓰면된다.

- main에서는 네이티브 코루틴 여러 개를 동시에 실행하는데, 먼저 ```asyncio.ensure_future``` 함수를 사용하여 태스크(asyncio.Task) 객체를 생성하고 리스트로 만듦

- ```변수 = await asyncio.gather(코루틴객체1, 코루틴객체2)``` 에서 asyncio.gather는 리스트가 아닌 위치 인수로 객체를 받으므로 태스크 객체를 리스트로 만들었다면 asyncio.gather(*futures)와 같이 리스트를 언패킹해서 넣어준다.

- 헷갈렸던 Asterisk(*)를 쓰는 이유는 위에 쓴거처럼 리스트여서 그렇구나!! 인수로 받아야하는 자리인데 리스트로 있으니까 그렇구나!!

- 이렇게 하면 오지게 빠르다.

- 끝.

<br>
<br>
[참고 : 코딩도장 - asyncio 사용하기](https://dojang.io/mod/page/view.php?id=2469)