---
layout: post
title:  "[Airflow] Airflow 마스터 클래스 인프런 강의 간단정리 "
subtitle: "Airflow"
categories: dev
tags: airflow
comments: true
published: true
---

Airflow 마스터 클래스 인프런 강의 간단하게 나중에 찾아보기 좋을 용도로 정리

---

### 섹션 1. Airflow설치

- 개발환경구성(전체설명, 사양 설명)
    - 개발 환경 플로우 이해(로컬에서 git push 해서 개발서버(리눅스)에서 git pull 받고 파일들 도커에 마운트해서 쓴다는 내용)
- 개발환경구성(파이썬 인터프리터설치)
    - airflow 워커 안에 들어가서 파이썬 버젼 알아내서 로컬에서 같은 환경을 만들어주는데 가상환경을 사용한다는 내용
- Airflow library 설치
    - 로컬서버에서는 가상환경에서 Airflow 라이브러리로 설치한다는 내용
    - 리눅스에서 파이썬 Airflow 라이브러리 설치시 그 자체로 Airflow 서비스 사용은 가능하나, pip install로 설치하면 저사양 아키텍처로 설치되며 Task도 한번에 1개씩만 실행 가능함.
  
### 섹션 2. 오퍼레이터 기본
- Bash operator DAG 만들기 & DAG 디렉토리 셋팅
    - docker-compose파일에서 dags 디렉토리를 마운트해줘야한다는 내용
    - 오퍼레이터 : 특정 행위를 할수 있는 기능을 모아 놓은 클래스(ex. Bash 오퍼레이터, Python 오퍼레이터 등)
    - Task : 오퍼레이터에서 객체화(인스턴트화)되어 DAG에서 실행 가능한 오브젝트
    - 스케쥴러는 DAG Parsing 후 DB에 정보저장, DAG 시작시간 결정
    - 워커는 실제 작업 수행
    - 웹 들어가서 사용법
        - 내가 만든 DAG 들어가서 CODE 봐보고
        - unpause 눌러주고
        - grid 눌러서 맨 왼쪽 긴 막대기 네모박스 클릭하면 지금까지 돌았던 내용들 나오고
        - 아래 각 task들 네모박스 클릭해서 log탭 보면 아래쪽에 결과값나온다.
- cron 스케줄 설명
    - 분 시 일 월 요일
- task 연결하기
    - Task 연결 방법 : >>, << 사용(Airflow 공식 추천방법), 함수로 사용하는 방법은 No 추천
- Bash Operator & 외부 쉘파일 수행하기
    - 쉘 스크립트가 필요한 이유 : 복잡한 로직 처리하는 경우(ex. sftp를 통해 파일을 받은 후 DB에 Insert&tar.gz으로 압축해두기)
    - Worker 컨테이너가 쉘 스크립트를 수행하려면 docker-compose파일에서 plugins 디렉토리에 쉘 스크립트를 저장하면됨(마운트)
- Email Operator로 메일 전송하기
    - 이메일 전송해주는 오퍼레이터
    - 구글에서 사전설정 작업(IMAP사용 및 앱비밀번호 셋팅 해주면됨)
  
### 섹션 3. Python 오퍼레이터
- Python 오퍼레이터 기본
    - PythonOperator(파이썬 함수 실행), BranchPythonOperator(파이썬함수 실행결과에 따라 task 선택적으로 실행)을 많이 씀
- 외부 파이썬 함수 수행하기
    - 파이썬 모듈 경로 이해해야함(dag에서 우리가 만든 외부함수를 import해와야하는데 import 경로를 어떻게 할까?)
    - sys.path로 하는 방법이 있지만, Airflow에서는 자동적으로 dags 폴더와 plugins 폴더를 sys.path에 추가함
    - 그래서 plugins 디렉토리에 외부함수 넣을 디렉토리 넣고 거기에 파이썬 파일 만들면 됨 (ex. /opt/airflow/plugins/common/common_func.py(def get_sftp() ~~))
- @task 데코레이터 사용하기
    - outer_func 안에 inner_func 예시(함수 실행전, 실행후 예시) 알려주면서 이걸 그냥 @outer_func으로 데코레이션 하는법 알려줌
    - 그냥 내가 기존에 쓰고싶었던 함수에 데코레이션만 붙이면 되니까 그대로 사용할수있고 많은 함수에 다 쉽게 넣을수 있다함
    - Task에서도 데코레이션으로 편하게 Task를 생성할수 있음
- 파이썬 함수 파라미터 이해
    - *args(튜플), **kwargs(딕셔너리) 설명해줌
- Python 오퍼레이터에 op_args로 변수 할당하기
    - 파이썬 오퍼레이션에 op_args 사용법 알려줌
- Python 오퍼레이터에 op_kwargs로 변수 할당하기
    - 파이썬 오퍼레이션에 op_kwargs 사용법 알려줌, op_args+op_kwargs 사용법도 알려줌
  
---
- 참고 : [Airflow 마스터 클래스 강의](https://www.inflearn.com/course/airflow-%EB%A7%88%EC%8A%A4%ED%84%B0-%ED%81%B4%EB%9E%98%EC%8A%A4) 