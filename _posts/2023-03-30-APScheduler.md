---
layout: post
title:  "[Python] APScheduler가 좋은점"
subtitle:   "Python"
categories: dev
tags: python
comments: true
published: true
---

APScheduler가 좋은점 소비하기 정리

---

### 결론 : APScheduler가 좋은점
- 작업을 동적으로 추가 삭제 할 수 있고
- 무한루프를 돌리지 않아도 코드를 비동기적으로 실행할수 있다.
- flask도 같이 띄울수도있다.
  
### 참고 블로그와 요약
- APScheduler 사용하기 ([참고](https://hello-bryan.tistory.com/216))
    - BlockingScheduler 와 BackgroundScheduler 코드 내용
  
- APScheduler 가이드 ([참고](https://apscheduler.readthedocs.io/en/latest/), [참고:한글정리블로그](https://blog.naver.com/PostView.nhn?blogId=imcjpak1k&logNo=221737678370&parentCategoryNo=&categoryNo=15&viewDate=&isShowPopularPosts=true&from=search))
    - 기본 컨셉, 다양한 스케쥴러 소개
  
- 그냥 기본 scheduler 함수 설명([참고](https://coding-kindergarten.tistory.com/164))
    - 가볍게 scheduler 사용할때 참고
  
- 쉽게 APScheduler 설명([참고](https://daco2020.tistory.com/111))
    - 특히 기존 스케줄러와 가장 큰 차이점이자 장점은 while과 같은 무한루프를 돌리지 않아도 코드를 비동기적으로 실행시킬 수 있다
    - 쉽게 말해 플라스크랑 스케쥴을 둘다 동시에 사용가능하다.
        - BlockingScheduler : 단일 스케쥴러
        - BackgroundScheduler : 다중 스케쥴러
  
- APScheduler 사용할때 실행하면 이상한 문구 나올때([참고](https://ffoorreeuunn.tistory.com/466))
    - ```timezone``` 설정하면됨.
  
- 파이썬 apscheduler 띄어놓고 킬하는 방법([참고](https://intrepidgeeks.com/tutorial/pthons-timing-operation-starts-and-ends-the-apscheduler))
    - 아래 코드처럼 하면 된다.
    ```
    python3 test.py &
    ps -e | grep python
    sudo kill -9 3344
    ```
  
- 파이썬으로 두 DB 간 테이블 동기화 코딩([참고](https://tzara.tistory.com/124))
    - 파이썬으로 두 DB 간 테이블 동기화 해야될때 사용하면 좋을듯
    - 스케쥴러를 이용해서 정기적으로 postgresql 의 테이블 데이터를 읽어서 MariaDB 에 넣어줌
    - 작업 처리 결과를 소스 테이블의 sync_flag 컬럼에 일괄 업데이트 (성공하면 'Y', 실패하면 'E')
  
- Job 제거 기능([참고](https://lemontia.tistory.com/508))
    - 블로그에는 schedule, apscheduler 내용이 나오는데,
    - 이미 등록되어 있는 Job 제거 기능을 활용하면 좋을것 같다.
  
- APScheduler는 큰 유연성을 가지고 있으며, 대부분의 요구사항을 충족([참고](https://velog.io/@sugar_ghost/Python-Schedule-vs-ASPcheduler))
    - Schedule은 가볍고 사용하기 쉽지만 작업 지속성을 지원하지 않으며, 작업을 동적으로 추가 삭제 할 수 없음

