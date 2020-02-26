---
layout: post
title:  "[코드정리]Web Scraping with Python(Ch03. 크롤링 시작하기)"
subtitle:   "PYTHON"
categories: pro
tags: python
comments: true
---

여러 사이트 이동하면서 크롤링하는것을 배운다.

---

[Web Scraping with Python Code](https://github.com/REMitchell/python-scraping) 에 대한 코드를 정리하였다.

# Ch03. 크롤링 시작하기


## 3.1 단일 도메인 내의 이동

- 여러사이트를 이동하는 스크레이퍼를 통해 크롤링
- 핵심은 재귀, 무한 반복
- 반드시 대역폭에 세심한 주의 기울여야, 타겟 서버 부하 줄일 방법 강구

### 링크 목록 가져오기
- 'a' -> 'href' 을 뽑아옴
- 문제 : 이상한 링크들이 따라옴
- 해결 : 
    - id가 bodyContent인 div안에 있다.
    - URL에는 세미콜론이 포함되어 있지 않다.
    - URL은 /wiki/로 시작한다.


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup 

html = urlopen('http://en.wikipedia.org/wiki/Kevin_Bacon')
bs = BeautifulSoup(html, 'html.parser')
for link in bs.find_all('a'):
    if 'href' in link.attrs:
        print(link.attrs['href'])
```

    /wiki/Wikipedia:Protection_policy#semi
    #mw-head
    #p-search
    /wiki/Kevin_Bacon_(disambiguation)
    /wiki/File:Kevin_Bacon_SDCC_2014.jpg
    /wiki/Philadelphia
    /wiki/Pennsylvania
    /wiki/Kyra_Sedgwick
    /wiki/Sosie_Bacon
    #cite_note-1
    /wiki/Edmund_Bacon_(architect)
    /wiki/Michael_Bacon_(musician)
    /wiki/Holly_Near
    http://baconbros.com/
    #cite_note-2
    #cite_note-actor-3
    /wiki/Footloose_(1984_film)
    

### Random Walk


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
import datetime
import random
import re

# 무작위 항목
random.seed(datetime.datetime.now())

# /wiki/<article_name> 형태인 URL을 받고 링크된 항목 URL 목록 전체를 반환
def getLinks(articleUrl):
    html = urlopen('http://en.wikipedia.org{}'.format(articleUrl))
    bs = BeautifulSoup(html, 'html.parser')
    return bs.find('div', {'id':'bodyContent'}).find_all('a', href=re.compile('^(/wiki/)((?!:).)*$'))

# 무작위 링크를 선택
links = getLinks('/wiki/Kevin_Bacon')
while len(links) > 0:
    newArticle = links[random.randint(0, len(links)-1)].attrs['href']
    print(newArticle)
    links = getLinks(newArticle)
```

    /wiki/David_Boreanaz
    /wiki/Hal_Jordan
    /wiki/Legion_of_Super-Villains
    /wiki/Lucy_Lane
    /wiki/Saturn_Girl
    /wiki/The_Lightning_Saga
    /wiki/Manhunters_(DC_Comics)
    /wiki/Hector_Hammond
    /wiki/Anti-Monitor
    /wiki/Superman
    /wiki/Eradicator_(comics)
    /wiki/Morgan_Edge
    /wiki/Teen_Titans
    /wiki/Gnarrk
    /wiki/Garth_(comics)
    /wiki/Silas_Stone
    


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    <ipython-input-2-ee18b17d20d4> in <module>
         15     newArticle = links[random.randint(0, len(links)-1)].attrs['href']
         16     print(newArticle)
    ---> 17     links = getLinks(newArticle)
 


## 3.2 전체 사이트 크롤링

- 이전 섹션은 링크에서 링크로 웹사이트를 무작위 이동했다.
- 여기선 모든 페이지를 어떤 시스템에 따라 분류해야할 떄 쓴다.
- 유용할때
    - 사이트맵 생성 : 사이트 내부 접근 권한 없을때 크롤러로 내부 링크를 모두 수집해서 사이트맵 생성
    - 데이터 수집 : 뉴스 기사 같은거 수집 할때

### Recursively crawling an entire site


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
import re

pages = set()
def getLinks(pageUrl):
    global pages
    html = urlopen('http://en.wikipedia.org{}'.format(pageUrl))
    bs = BeautifulSoup(html, 'html.parser')
    for link in bs.find_all('a', href=re.compile('^(/wiki/)')):
        if 'href' in link.attrs:
            if link.attrs['href'] not in pages:
                #We have encountered a new page
                newPage = link.attrs['href']
                print(newPage)
                pages.add(newPage)
                getLinks(newPage)
getLinks('')
```

    /wiki/Wikipedia
    /wiki/Wikipedia:Protection_policy#semi
    /wiki/Wikipedia:Requests_for_page_protection
    


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    <ipython-input-3-94457f9052fa> in <module>
         16                 pages.add(newPage)
         17                 getLinks(newPage)
    ---> 18 getLinks('')
    


### Crawling across the Internet
- 페이지 제목, 첫번째 문단, 편집 페이지를 가르키는 링크를 수집하는 스크레이퍼를 만들어 보자
- 가장 먼저 할일은 사이트의 페이지 몇개를 살펴보며 패턴는 찾음
- 데이터들을 print 하게끔 만듦


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
import re

pages = set()
def getLinks(pageUrl):
    global pages
    html = urlopen('http://en.wikipedia.org{}'.format(pageUrl))
    bs = BeautifulSoup(html, 'html.parser')
    try:
        print(bs.h1.get_text())
        print(bs.find(id ='mw-content-text').find_all('p')[0])
        print(bs.find(id='ca-edit').find('span').find('a').attrs['href'])
    except AttributeError:
        print('This page is missing something! Continuing.')
    
    for link in bs.find_all('a', href=re.compile('^(/wiki/)')):
        if 'href' in link.attrs:
            if link.attrs['href'] not in pages:
                #We have encountered a new page
                newPage = link.attrs['href']
                print('-'*20)
                print(newPage)
                pages.add(newPage)
                getLinks(newPage)
getLinks('')
```

    Main Page
    <p><i><b><a href="/wiki/The_Cabinet_of_Dr._Caligari" title="The Cabinet of Dr. Caligari">The Cabinet of Dr. Caligari</a></b></i> is a German <a href="/wiki/Silent_film" title="Silent film">silent</a> <a href="/wiki/Horror_film" title="Horror film">horror film</a>, first released on 26 February 1920. It was directed by <a href="/wiki/Robert_Wiene" title="Robert Wiene">Robert Wiene</a> and written by <a href="/wiki/Hans_Janowitz" title="Hans Janowitz">Hans Janowitz</a> and <a href="/wiki/Carl_Mayer" title="Carl Mayer">Carl Mayer</a>. Considered the quintessential work of <a href="/wiki/German_Expressionism" title="German Expressionism">German Expressionist</a> cinema, it tells the story of an insane hypnotist (<a href="/wiki/Werner_Krauss" title="Werner Krauss">Werner Krauss</a>) who uses a <a href="/wiki/Sleepwalking" title="Sleepwalking">somnambulist</a> (<a href="/wiki/Conrad_Veidt" title="Conrad Veidt">Conrad Veidt</a>) to commit murders. The film features a dark and twisted visual style. The sets have sharp-pointed forms, oblique and curving lines, and structures that lean and twist in unusual angles. The film's design team, <a href="/wiki/Hermann_Warm" title="Hermann Warm">Hermann Warm</a>, <a href="/wiki/Walter_Reimann" title="Walter Reimann">Walter Reimann</a> and <a href="/wiki/Walter_R%C3%B6hrig" title="Walter Röhrig">Walter Röhrig</a>, recommended a fantastic, graphic style over a naturalistic one. With a violent and  insane authority figure as its antagonist, the film expresses the theme of brutal and irrational authority. Considered a classic, it helped draw worldwide attention to the artistic merit of German cinema and had a major influence on American films, particularly in the genres of horror and <a href="/wiki/Film_noir" title="Film noir">film noir</a>. (<b><a href="/wiki/The_Cabinet_of_Dr._Caligari" title="The Cabinet of Dr. Caligari">Full article...</a></b>) 
    </p>
    This page is missing something! Continuing.
    --------------------
    /wiki/Wikipedia
    Wikipedia
    <p class="mw-empty-elt">
    </p>
    This page is missing something! Continuing.
    --------------------
    /wiki/Wikipedia:Protection_policy#semi
    Wikipedia:Protection policy
    <p class="mw-empty-elt">
    </p>
    This page is missing something! Continuing.
    --------------------
    /wiki/Wikipedia:Requests_for_page_protection
    Wikipedia:Requests for page protection
    <p>This page is for requesting that a page, file or template be <strong>fully protected</strong>, <strong>create protected</strong> (<a href="/wiki/Wikipedia:Protection_policy#Creation_protection" title="Wikipedia:Protection policy">salted</a>), <strong>extended confirmed protected</strong>, <strong>semi-protected</strong>, added to <strong>pending changes</strong>, <strong>move-protected</strong>, <strong>template protected</strong>, <strong>upload protected</strong> (file-specific), or <strong>unprotected</strong>. Please read up on the <a href="/wiki/Wikipedia:Protection_policy" title="Wikipedia:Protection policy">protection policy</a>. Full protection is used to stop edit warring between multiple users or to prevent vandalism to <a href="/wiki/Wikipedia:High-risk_templates" title="Wikipedia:High-risk templates">high-risk templates</a>; semi-protection and pending changes are usually used only to prevent IP and new user vandalism (see the <a href="/wiki/Wikipedia:Rough_guide_to_semi-protection" title="Wikipedia:Rough guide to semi-protection">rough guide to semi-protection</a>); and move protection is used to stop <a href="/wiki/Wikipedia:Page-move_war" title="Wikipedia:Page-move war">page-move wars</a>. Extended confirmed protection is used where semi-protection has proved insufficient (see the <a href="/wiki/Wikipedia:Rough_guide_to_extended_confirmed_protection" title="Wikipedia:Rough guide to extended confirmed protection">rough guide to extended confirmed protection</a>)
    </p>
    This page is missing something! Continuing.
    --------------------
    /wiki/Wikipedia:Protection_policy#move
    Wikipedia:Protection policy
    <p class="mw-empty-elt">
    </p>
    This page is missing something! Continuing.
    --------------------
    /wiki/Wikipedia:Lists_of_protected_pages
    


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    <ipython-input-4-ac53a9af3c4b> in <module>
         24                 pages.add(newPage)
         25                 getLinks(newPage)
    ---> 26 getLinks('')
    



## 3.3 인터넷 크롤링

### Crawling across the Internet
- 외부 링크까지도 가져오기


```python

from urllib.request import urlopen
from urllib.parse import urlparse
from bs4 import BeautifulSoup
import re
import datetime
import random

pages = set()
random.seed(datetime.datetime.now())

#Retrieves a list of all Internal links found on a page
def getInternalLinks(bs, includeUrl):
    includeUrl = '{}://{}'.format(urlparse(includeUrl).scheme, urlparse(includeUrl).netloc)
    internalLinks = []
    #Finds all links that begin with a "/"
    for link in bs.find_all('a', href=re.compile('^(/|.*'+includeUrl+')')):
        if link.attrs['href'] is not None:
            if link.attrs['href'] not in internalLinks:
                if(link.attrs['href'].startswith('/')):
                    internalLinks.append(includeUrl+link.attrs['href'])
                else:
                    internalLinks.append(link.attrs['href'])
    return internalLinks
            
#Retrieves a list of all external links found on a page
def getExternalLinks(bs, excludeUrl):
    externalLinks = []
    #Finds all links that start with "http" that do
    #not contain the current URL
    for link in bs.find_all('a', href=re.compile('^(http|www)((?!'+excludeUrl+').)*$')):
        if link.attrs['href'] is not None:
            if link.attrs['href'] not in externalLinks:
                externalLinks.append(link.attrs['href'])
    return externalLinks

def getRandomExternalLink(startingPage):
    html = urlopen(startingPage)
    bs = BeautifulSoup(html, 'html.parser')
    externalLinks = getExternalLinks(bs, urlparse(startingPage).netloc)
    if len(externalLinks) == 0:
        print('No external links, looking around the site for one')
        domain = '{}://{}'.format(urlparse(startingPage).scheme, urlparse(startingPage).netloc)
        internalLinks = getInternalLinks(bs, domain)
        return getRandomExternalLink(internalLinks[random.randint(0,
                                    len(internalLinks)-1)])
    else:
        return externalLinks[random.randint(0, len(externalLinks)-1)]
    
def followExternalOnly(startingSite):
    externalLink = getRandomExternalLink(startingSite)
    print('Random external link is: {}'.format(externalLink))
    followExternalOnly(externalLink)
            
followExternalOnly('http://oreilly.com')
```

    Random external link is: https://oreilly.formulated.by/scme-miami-2020/
    Random external link is: https://www.linkedin.com/in/keith-carswell-71aa5299/
    


    ---------------------------------------------------------------------------

    HTTPError                                 Traceback (most recent call last)

    <ipython-input-11-a23b136d3234> in <module>
         52     followExternalOnly(externalLink)
         53 
    ---> 54 followExternalOnly('http://oreilly.com')
    

   

### Collects a list of all external URLs found on the site
- 저장하기


```python
allExtLinks = set()
allIntLinks = set()


def getAllExternalLinks(siteUrl):
    html = urlopen(siteUrl)
    domain = '{}://{}'.format(urlparse(siteUrl).scheme,
                              urlparse(siteUrl).netloc)
    bs = BeautifulSoup(html, 'html.parser')
    internalLinks = getInternalLinks(bs, domain)
    externalLinks = getExternalLinks(bs, domain)

    for link in externalLinks:
        if link not in allExtLinks:
            allExtLinks.add(link)
            print(link)
    for link in internalLinks:
        if link not in allIntLinks:
            allIntLinks.add(link)
            getAllExternalLinks(link)


allIntLinks.add('http://oreilly.com')
getAllExternalLinks('http://oreilly.com')
```

    https://www.oreilly.com
    https://www.oreilly.com/sign-in.html
    https://www.oreilly.com/online-learning/try-now.html
    https://www.oreilly.com/online-learning/index.html
    https://www.oreilly.com/online-learning/individuals.html
    https://www.oreilly.com/online-learning/teams.html
    https://www.oreilly.com/online-learning/enterprise.html
    https://www.oreilly.com/online-learning/government.html
    https://www.oreilly.com/online-learning/academic.html
    https://www.oreilly.com/online-learning/features.html
    https://www.oreilly.com/online-learning/custom-services.html
    https://www.oreilly.com/online-learning/pricing.html
    https://www.oreilly.com/conferences/
   
    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    <ipython-input-12-fe3fc94e1803> in <module>
         23 
         24 allIntLinks.add('http://oreilly.com')
    ---> 25 getAllExternalLinks('http://oreilly.com')
    



## 3.4 스크래파이를 사용한 크롤링

- 스크래파이라는 반복작업을 줄여주는 라이브러리도 존재함
