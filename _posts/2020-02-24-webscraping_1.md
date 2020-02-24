---
layout: post
title:  "[코드정리]Web Scraping with Python(Ch01. 첫 번째 웹 스크레이퍼)"
subtitle:   "PYTHON"
categories: pro
tags: python
comments: true
---
웹 크롤링을 할 일이 생겨서 차근차근 공부해 보려한다. 

---

[Web Scraping with Python Code](https://github.com/REMitchell/python-scraping) 에 대한 코드를 정리하였다.

# Ch01. 첫 번째 웹 스크레이퍼


- 페이지의 HTML 코드 전체를 가져오기


```python
from urllib.request import urlopen

html = urlopen('http://pythonscraping.com/pages/page1.html')
print(html.read())
```

    b'<html>\n<head>\n<title>A Useful Page</title>\n</head>\n<body>\n<h1>An Interesting Title</h1>\n<div>\nLorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.\n</div>\n</body>\n</html>\n'
    

- BeautifulSoup 객체 구조(html -> body -> h1)으로 들어가기 
    - bs.h1
    - bs.html.body.h1
    - bs.body.h1
    - bs.html.h1
    html, body로 두 단계만큼 중첩 되어 있지만 이 4가지 중 무엇을 써도 결과 같다. 


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup

html = urlopen('http://www.pythonscraping.com/pages/page1.html')
bs = BeautifulSoup(html.read(), 'html.parser')
print(bs.html.body.h1)
```

    <h1>An Interesting Title</h1>
    

- urlopen만 하면 문제가 생길 수 있다.
    1. 페이지를 찾을 수 없거나, URL 해석에 에러
    2. 서버를 찾을 수 없는 경우


```python
from urllib.request import urlopen
from urllib.error import HTTPError
from urllib.error import URLError

try:
    html = urlopen("https://pythonscrapingthisurldoesnotexist.com")
except HTTPError as e:
    print("The server returned an HTTP error")
except URLError as e:
    print("The server could not be found!")
else:
    print(html.read())
```

    The server could not be found!
    

- 어떤 문제가 있으면 None 객체를 반환하는 getTitle 함수를 만든다.
- 두 개의 에러에 대해 Try문을 캡슐화 한다.
- 예외 처리를 철저하게 만들면 웹 스크레이퍼를 쉽게 만들수 있다.


```python
from urllib.request import urlopen
from urllib.error import HTTPError
from bs4 import BeautifulSoup


def getTitle(url):
    try:
        html = urlopen(url)
    except HTTPError as e:
        print(e)
        return None
    try:
        bsObj = BeautifulSoup(html, "html.parser")
        title = bsObj.body.h1
    except AttributeError as e:
        return None
    return title

title = getTitle("http://www.pythonscraping.com/exercises/exercise1.html")
if title == None:
    print("Title could not be found")
else:
    print(title)
    
```

    <h1>An Interesting Title</h1>
    
