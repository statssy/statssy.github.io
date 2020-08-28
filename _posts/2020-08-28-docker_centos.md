---
layout: post
title:  "[도커] Window에서 도커 CentOS 컨테이너 실행하기"
subtitle:   "Docker"
categories: dev
tags: docker
comments: true
---

저번에 도커에 대한 강의를 들었지만 까먹기도했고 도커에서 CentOS 컨테이너를 실행해야 해서 정리겸 핵심만 정리한다.

---

### 도커에 CentOS 설치 및 컨테이너 실행
- docker 설치 [참고 : 도커 설치](https://hello-bryan.tistory.com/159)
- 로그인하기 : ```> docker login```
- Pull image```> docker pull centos```
- image 확인(이미지 목록 보기. 잘깔렸나 보기) : ```> docker images```
- container 생성(생성 동시에 실행) : ```> docker run -i -t --name centos-container centos:latest bin/bash```
(--name 뒤는 container이름. 여기서 bin/bash라는 실행파일을 실행해야하기 때문에.)
- container에서 나가는 법 : 1) 종료하고 나가기 : ```> exit```, 2) 종료하지 않고 나가기 Ctrl+p 누른후 Ctrl+q
- 도커 STATUS 확인 : ```> docer ps -a```
- container 실행(실행 따로 접속 따로) : ```> docker start centos-container``` or ```> docker start 컨테이너아이디(일부만해도됨)```
- container 접속 : ```> docker attach centos-container``` or ```> docker attach 컨테이너아이디```
(당연히 컨테이너 종료하지 않고 나가기 했으면 attach만 하면 된다.)

### 컨테이너에 git 깔기, clone하기
- container 에서 git 깔기 : ```> yum install git```
- container 만들어져있는거 clone 하기 : ```> docker commit 원root옆문자 이름``` ```예시) > docker commit f7b63~ dms-centos```

[참고1 : 도커 실행](https://hello-bryan.tistory.com/160)  

[참고2 : 도커 클론](https://www.youtube.com/watch?v=Hul3MuMP-tM)