---
layout: post
title:  "[코드정리]Web Scraping with Python(Ch02. 고급 HTML 분석)"
subtitle:   "WebScraping"
categories: data
tags: scrap
comments: true
---

HTML에서 어떻게 안에 내용들을 찾아 오는가에 대한 코드를 정리한다.

---

[Web Scraping with Python Code](https://github.com/REMitchell/python-scraping) 에 대한 코드를 정리하였다.

# Ch02. 고급 HTML 분석


## BeautifulSoup로 find()나 findAll()을 호출해서 함수 이름과 속성에 따라 태그 찾는 방법

- 예를들어 아래와 같은 녹색 태그들을 리스트로 추출할 수 있다.
```
<span class="green">Anna
Pavlovna Scherer</span>
```


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
html = urlopen('http://www.pythonscraping.com/pages/warandpeace.html')
bs = BeautifulSoup(html, "html.parser")

nameList = bs.findAll('span', {'class': 'green'})
for name in nameList[0:5]: # 몇 개만
    print(name.get_text())
```

    Anna
    Pavlovna Scherer
    Empress Marya
    Fedorovna
    Prince Vasili Kuragin
    Anna Pavlovna
    St. Petersburg
    

### find()와 findAll()
- find()와 findAll()의 차이는 limit 매개변수가 있는지에 차이
- 보통 처음 tag와 attributes만 씀 ```bs.findAll('span', {'class': 'green'})```
- tag : 여러개 가능 ex) 여러개의 헤더
- attributes : 딕셔너리 받음
- recursive : 문서 얼마나 깊이 찾을지
- text : 태그 속성이 아니라 텍스트가 얼마나 나오는지
- limit 매개변수는 페이지의 항목 처음 몇 개에만 관심 있을 때 사용
- find는 findAll의 limit 1과 같음
- keyword : 특성 속성이 포함된 태그를 선택


```python
titles = bs.find_all(['h1', 'h2','h3','h4','h5','h6'])
print([title for title in titles])
```

    [<h1>War and Peace</h1>, <h2>Chapter 1</h2>]
    


```python
nameList = bs.find_all(text='the prince')
print(len(nameList))
```

    7
    

## 트리 이동 : 위치를 기준으로 태그 찾기 

- 자식과 자손 : 자식은 한태그 아래 자손은 몇 단계 아래
    - 자식만 찾을 때는 .children을 사용



```python
# 테이블 정보 가져오기 
from urllib.request import urlopen
from bs4 import BeautifulSoup

html = urlopen('http://www.pythonscraping.com/pages/page3.html')
bs = BeautifulSoup(html, 'html.parser')

for child in bs.find('table',{'id':'giftList'}).children:
    print(child)
```

    
    
    <tr><th>
    Item Title
    </th><th>
    Description
    </th><th>
    Cost
    </th><th>
    Image
    </th></tr>
    
    
    <tr class="gift" id="gift1"><td>
    Vegetable Basket
    </td><td>
    This vegetable basket is the perfect gift for your health conscious (or overweight) friends!
    <span class="excitingNote">Now with super-colorful bell peppers!</span>
    </td><td>
    $15.00
    </td><td>
    <img src="../img/gifts/img1.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift2"><td>
    Russian Nesting Dolls
    </td><td>
    Hand-painted by trained monkeys, these exquisite dolls are priceless! And by "priceless," we mean "extremely expensive"! <span class="excitingNote">8 entire dolls per set! Octuple the presents!</span>
    </td><td>
    $10,000.52
    </td><td>
    <img src="../img/gifts/img2.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift3"><td>
    Fish Painting
    </td><td>
    If something seems fishy about this painting, it's because it's a fish! <span class="excitingNote">Also hand-painted by trained monkeys!</span>
    </td><td>
    $10,005.00
    </td><td>
    <img src="../img/gifts/img3.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift4"><td>
    Dead Parrot
    </td><td>
    This is an ex-parrot! <span class="excitingNote">Or maybe he's only resting?</span>
    </td><td>
    $0.50
    </td><td>
    <img src="../img/gifts/img4.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift5"><td>
    Mystery Box
    </td><td>
    If you love suprises, this mystery box is for you! Do not place on light-colored surfaces. May cause oil staining. <span class="excitingNote">Keep your friends guessing!</span>
    </td><td>
    $1.50
    </td><td>
    <img src="../img/gifts/img6.jpg"/>
    </td></tr>
    
    
    


```python
# 형제 다루기
# 테이블 정보에서 맨 상단 타이틀 행은 안가져오는 next_sibling
from urllib.request import urlopen
from bs4 import BeautifulSoup

html = urlopen('http://www.pythonscraping.com/pages/page3.html')
bs = BeautifulSoup(html, 'html.parser')

for sibling in bs.find('table', {'id':'giftList'}).tr.next_siblings:
    print(sibling)
```

    
    
    <tr class="gift" id="gift1"><td>
    Vegetable Basket
    </td><td>
    This vegetable basket is the perfect gift for your health conscious (or overweight) friends!
    <span class="excitingNote">Now with super-colorful bell peppers!</span>
    </td><td>
    $15.00
    </td><td>
    <img src="../img/gifts/img1.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift2"><td>
    Russian Nesting Dolls
    </td><td>
    Hand-painted by trained monkeys, these exquisite dolls are priceless! And by "priceless," we mean "extremely expensive"! <span class="excitingNote">8 entire dolls per set! Octuple the presents!</span>
    </td><td>
    $10,000.52
    </td><td>
    <img src="../img/gifts/img2.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift3"><td>
    Fish Painting
    </td><td>
    If something seems fishy about this painting, it's because it's a fish! <span class="excitingNote">Also hand-painted by trained monkeys!</span>
    </td><td>
    $10,005.00
    </td><td>
    <img src="../img/gifts/img3.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift4"><td>
    Dead Parrot
    </td><td>
    This is an ex-parrot! <span class="excitingNote">Or maybe he's only resting?</span>
    </td><td>
    $0.50
    </td><td>
    <img src="../img/gifts/img4.jpg"/>
    </td></tr>
    
    
    <tr class="gift" id="gift5"><td>
    Mystery Box
    </td><td>
    If you love suprises, this mystery box is for you! Do not place on light-colored surfaces. May cause oil staining. <span class="excitingNote">Keep your friends guessing!</span>
    </td><td>
    $1.50
    </td><td>
    <img src="../img/gifts/img6.jpg"/>
    </td></tr>
    
    
    


```python
# 부모 다루기
# ../img/gifts/img1.jpg 이미지가 나태내는 객체의 가격을 출력
# 이미지 전의 부모가 가격임.
from urllib.request import urlopen
from bs4 import BeautifulSoup

html = urlopen('http://www.pythonscraping.com/pages/page3.html')
bs = BeautifulSoup(html, 'html.parser')
print(bs.find('img',
              {'src':'../img/gifts/img1.jpg'})
      .parent.previous_sibling.get_text())
```

    
    $15.00
    
    

## 정규 표현식

- 예를들어, 이미지를 가지고 오고싶을 때, ```find("img")```라고 쓰고 찾으면 이상한 로고들이 들어올 수 있다. 
- 그러므로 해결책은 태그 자체를 식별하는 무언가를 찾는다. 예를들어 파일 경로로 찾는다.
- import re로 정규 표현식을 임포트 한다.


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
import re

html = urlopen('http://www.pythonscraping.com/pages/page3.html')
bs = BeautifulSoup(html, 'html.parser')
images = bs.find_all('img', {'src':re.compile('\.\.\/img\/gifts/img.*\.jpg')})
for image in images: 
    print(image['src'])
```

    ../img/gifts/img1.jpg
    ../img/gifts/img2.jpg
    ../img/gifts/img3.jpg
    ../img/gifts/img4.jpg
    ../img/gifts/img6.jpg
    

## 속성에 접근하기 
: 속성에 관심 있을때 ex) Url이 href 속성에 들어있는 ```<a>```태그, 이미지가 src속성에 있는 ```<img>```태그

## 람다 표현식
: ```soup.findAll(lambda tag: len(tag.attrs) == 2)``` 이렇게 속성이 2개인 태그를 모두 가져오는 방법 쓸때 쓴다


```python

```
