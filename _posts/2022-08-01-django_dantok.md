---
layout: post
title:  "[Django] 파이썬 장고로 단톡방 만들기"
subtitle:   "Python"
categories: dev
tags: python
comments: true
published: true
---

당장 장고 프로젝트 할건 아닌데, 주말에 시간도 남고 해서 그냥 이런게 있었구나 기억용으로 써놓는다.(그래서 정확하게 안썼다.)

---
  
[바보도 하는 파이썬 장고](https://www.youtube.com/watch?v=bAEecOVKSfY) 를 참고했다.  
최대한 정확한 정리보단, 이런게 있었구나 용도로 써야, 시간도 덜걸리고 스트레스도 덜한거 같다.  
  
### 2 장고 설치와 기본 셋업 - 바보도 하는 파이썬 장고 (Python Django) - 완전 기초부터 테마 적용 프로젝트까지

- 파이썬 설치
- 가상환경 (python -m venv env)
- 가상환경 activate
- 장고 설치(pip install django)
- 장고 명령어 리스트 참고(django-admin)
    - makemigrations
    - migrate
    - startproject
- django-admin startproject {프로젝트이름}
    - (django-admin startproject dantok)
- python manage.py runserver
- 127.0.0.1:8000 하면 들어가진다.
- vscode로 쟝고 프로젝트 열기
    - 구조들이 보인다.
- manage.py 로 보기. : 젤 중요한 파일
- urls.py : 라우팅
- settings.py
    - ALLOWED_HOSTS가 비어있어서 일단 어떤 호스트로도 가능하다.
    - DATABASES : 일단은 장고가 만들어준 sqlite로 세팅되어 있다.
    - INSTALLED_APPS : 각각 기능들(모듈), 추가해야한다. 네이버 블로그 카페 이런식으로 앱으로 만드는거다. 
- 앱 만들기
    - python manage.py startapp {앱이름}
    - python manage.py startapp basic
    - 그러면 앱이 만들어진다.
    - setting.py 에서 INSTALLED_APPS에 룰에 맞게 basicconfig로 넣어주면된다.(그러면 앱 연결)

### 3 urls와 views - 바보도 하는 파이썬 장고 (Python Django) - 완전 기초부터 테마 적용 프로젝트까지
- url과 view 에 관해서 해본다.
- urls.py
    - urlpatterns으로 간단하게 라우팅 테스트해보기
        - 127.0.0.1:8000/admin으로 들어가면 나온다.
        - path를 계속 추가하면 된다.
        - path('', views.index) 라고 치고 views.py 하나 만들어보자. 간단하게. 근데 이러면 너무 복잡하다.
        - basic(앱이름)안에 views에 들어가서 만들어준다. urls도 만들어준다.
        - 다 만들면 dantok(메인)에 가서 include('basic.urls')를 추가해준다.

### 4 장고 템플릿 기본 1 - 바보도 하는 파이썬 장고 (Python Django) - 완전 기초부터 테마 적용 프로젝트까지
- templates 디렉토리를 만들어주고
- html을 작성해서 넣어준다.
- HttpResponse 대신 render를 이용해서 템플릿을 가져오게 한다.
- BASE_DIR/TEMPLATES 를 setting.py에 템플릿에 추가

### 5 장고 템플릿 엔진 2 - 바보도 하는 파이썬 장고 (Python Django) - 완전 기초부터 테마 적용 프로젝트까지
- include를 사용해서 index.html을 만들고 하는 방법도 있고
- 새롭게 main.html 파일을 만들어서 ht까지만 치면 html5-boilerplate 라고 나오는데 이게 html에 기본틀을 자동으로 만들어준다.
- main으로 와꾸를 잡고 extends를 사용하는 방법이 있다.
    - 그럼 다른 html을 만들어서 extend랑 block을 써서 main으로 와꾸 잡으면 다 적용된다.
- 변하는 데이터 라면?
    - docs.djangoproject.com으로 들어가서 template 들어가면 자세히 나온다. 중괄호로 막 만들수있다.
    - 만약에 html을 앱으로 옮기면 render 안에 옮긴앱으로 경로를 바꿔주면된다.
- 변형된 주소처리 home/1, home/2 이렇게
    - str 지정하고 파라미터로 쓰면 된다.
    - href로 목차 들어가기 바꾸기 가능
