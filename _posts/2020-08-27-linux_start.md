---
layout: post
title:  "[리눅스] RestAPI 공부전, 홈페이지 만들때 전체적인 맥락 알기"
subtitle:   "API"
categories: dev
tags: api
comments: true
---

리눅스 명령어를 숨쉬듯 써야되니까 정리 해놓자.

---

### 리눅스가 뭐지?
- UNIX 운영체제 기반으로 만들어진 운영체제(서버용으로 많이 사용된다.)
- 센토스(서버용), 우분투(데스크톱) (사실 두개 차이가 없다 거의)


### 리눅스 설치 - CentOS7
- VirtualBox 설치
- CentOS iso 다운받아서 VirtualBox에 올리기

[참고 :양주종의 코딩스쿨](https://www.youtube.com/watch?v=WUtNyOuWjOQ)


### 리눅스 명령어 정리

- pwd : 현재 디렉토리
- cd, cd .. : 경로 이동, 전 경로
- ls, ls -l : 목록, 목록 상세
- cp, cp -r : 복사, 디렉토리 복사
- mv : 위치이동, 이름변경
- mkdir, mkdir -p : 디렉토리 생성, 하위 디렉토리까지 생성(e.g. mkdir -p a/b/c/d/e)
- rm, rm -f,  rm -r, rm -rf : 삭제, 디렉토리 삭제, 여부x 삭제, 디렉토리 여부x 삭제
- touch : 파일만들기
- cat, head, tail, more : 텍스트 파일 보기, 앞열보기, 뒷열보기, 페이지단위로보기

[참고 : 이것이 리눅스다](https://www.youtube.com/watch?v=DkpmcTRGmt4)