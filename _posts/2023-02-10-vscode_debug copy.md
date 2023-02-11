---
layout: post
title:  "[VSCODE] VSCODE에서 디버깅 요약"
subtitle:   "Python"
categories: dev
tags: python
comments: true
published: true
---

VSCODE에서 디버깅 기능을 내식대로 쓰다가 유튜브에 정리하신분이 있어서 간단히 정리한다.

---
  
### VSCODE에서 디버깅하기
   
도커 컨테이너에 Flask API로 구동되는걸 어떻게 디버깅해야 편하게 할까? 일단 기초부터 차근차근 해보자.  
    
[참고 영상](https://www.youtube.com/watch?v=_1HM6MJMYPw)에서 영상보고 활용 팁 이해  
[참고 블로그](https://reese-dev.netlify.app/etc/vscode_debugging/)에서 이해 도움

  
- Watch
    - 표현식을 입력하면 breakpoints에서의 값을 보여줌
   
- Call Stack
    - 어떤 함수가 어디서 실행했는지 그리고 어떤 함수 안에서 어떤 함수가 호출되었는지 추적 가능
    - 호출(실행) 순서는 아래서 위로 간다.
   
- Step over
    - breakpoint 다음 라인으로 이동
   
- Step into
    - 다음 라인으로 넘어가긴 하는데, 다음라인이 함수이면 함수 내부로 들어간다.
   
- Step out
    - 현재 함수의 나머지 부분을 실행시키고 리턴에서 멈춘다.
   
- breakpoint 팁
    - 실행 > 모든 breakpoint 제거 있다.
    - 활성화 비활성화도 가능하다.(그러면 회색동그라미가 된다.)
  
다음 블로그에서는 [다니얼 아저씨의 Developing a Flask app with Visual Studio Code](https://www.youtube.com/watch?v=P1PCS6puIyA&t=3090s)를 보고 정리해보려고 한다.
