---
layout: post
title:  "[코딩사전] REST API가 뭔가요?"
subtitle:   "API"
categories: dev
tags: api
comments: true
---

API를 공부해야 되어 일단 REST API가 뭔지 기본을 잡고 가보도록 하자.


### REST API가 뭔가요?


- 자세한 내용은 [그런 REST API로 괜찮은가](https://slides.com/eungjun/rest) 를 보면 잘 정리 되어있다.
- REST API는 정보들이 주고받아지는데 있어서 일종의 형식(e.g 우체국 송장)
- API란? : 기계와 기계, 소프트웨어와 소프트웨어 사이에서 수많은 요청과 교환할 소통의 창구
    - 예를 들어 기상정보가 관리되는 기상청 서버 <-> 앱들이 실시간으로 날씨정보를 받아간다.
    - ```"date:191031 | place:seoul | which:temperature" <-> "17degree"```
- 이렇게 지정된 형식으로 요청, 명령을 받을 수 있는 수단을 API(Application Programming Interface) 라고 한다.


- REST의 특성은 각 요청이 어떤 동작이나 정보를 위한 것인지를 그 요청의 모습 자체로 추론 가능
    - e.g) ```https://(도메인)/classes/2/students/15``` 같이 
- C.R.U.D(Create, Read, Update, Delete)를 해야한다. 조회만 할순 없지않는가
- GET, DELETE, POST, PUT, PATCH가 있다.
    - POST, PUT, PATCH : 이건 body란 주머니가 있어서 GET이나 DELETE보다 비교적 안전하게 감춰서 실어 보낼수 있다.
        - GET : Read하는거
        - POST : Create 추가하기
        - PUT : Update(통채로)
        - PATCH : Update(일부만)
        - DELETE : 삭제

    
[참고 사이트(얄팍한 코딩사전)](https://www.youtube.com/watch?v=iOueE9AXDQQ)