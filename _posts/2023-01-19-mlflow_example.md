---
layout: post
title:  "[MLflow] MLflow 간단 설명 + Example 따라해보기"
subtitle:   "Python"
categories: dev
tags: python
comments: true
published: true
---

MLflow Example을 한번 해보자. 모델서빙 기능은 미친게 확실하다.

---
   
## MLflow란? 머신러닝 라이플 사이클 관리
- [참고 블로그](https://lsjsj92.tistory.com/625)

- 주요기능
    - MLflow Tracking : 머신러닝 모델 학습 시킬때 파라미터, training 끝나고 메트릭(평가지표) 결과등을 로깅하고 그 결과를 Web UI로 확인 가능
    - MLflow Project : 아나콘다나 도커 등을 사용해서 만들어 둔 모델을 reproducible하고 실행할 수 있도록 코드 패키지 형식으로 지원. 이 형식으로 환경을 재사용할 수 있음
    - MLflow Models : 동일한 모델을 Docker, Spark, AWS 등에서 쉽게 배치할수 있도록 지원
    - MLflow Model Registry : MLflow 모델의 전체 라이프사이클을 공동으로 관리하기 위한 centralized model store, Set of API, UI
  
- 예제코드 
    - MLflow Tracking
        - main.py
            - model.py 에서 넘겨온 model 정보를 받아 mlflow에서 제공해주는 log_metric, log_param, log_model 등에 정보를 주입    
        - model.py
            - 텐서플로와 사이킷런 모델가지고 데이터 훈련, 트레이닝된 모델과 하이퍼파라미터 정보를 main.py에 리턴
        - Tracking
            - 트레이닝하고 그결과값을 로깅 처리후 관리
            - log_metric : 평가지표를 로깅
            - log_param : 파라미터값 저장
            - log_model : 모델 저장
  
- Project & Package
    - 머신러닝 모델을 reproduct 하고 실행할 수 있는 코드 패키지 형식으로 지원
    - Tracking 부분은 로깅하고 관리라면 Package는 그 모델을 재생산하고 배포할 수 있도록 지원
    - Tracking 결과로 Artifact쪽에 conda.yaml이 존재하는데 이 파일을 함께 활용
    - 이 블로그에서는 mlflow-env라는 디렉토리 생성해서 Project&Package를 진행하여 앞서 진행했던 파일을 전부 이동시킴.(단 여기서 2가지의 파일을 새로 추가)
        - conda.yaml : conda에 dependency에 pip 요소와 python 버젼 명시
        - MLproject : MLflow 실행시 각종 환경요소, 파라미터, 커맨드 명령어 등을 정의
    - 이렇게 만들어진 파일들을 활용해 mlflow를 실핼시켜주면 됨
    - test 환경도 설정 가능

---

## example 테스트 참고
   
### 시작 전에, 읽어볼만한 것
- mlflow quickstart 한글 설명
    - 참고 : https://dailyheumsi.tistory.com/257
  
- mlflow ui 가 안될시(도커에서는 이렇게 해야함)
    - 참고 : https://groups.google.com/g/mlflow-users/c/7cezJGzfnic
    - ```mlflow server -h 0.0.0.0```
  
### mlflow sklearn_elasticnet_wine 테스트(UI 잘 띄어지는지 확인용)
- 참고 : https://dailyheumsi.tistory.com/257
- MLflow 프로젝트
    - example 디렉토리에서 입력
    - --no-conda 변경됨 (참고:https://stackoverflow.com/questions/60158939/deploying-mlflow-model-without-conda-environment)
    - ```mlflow run sklearn_elasticnet_wine -P alpha=0.5 --env-manager=local```
   
### 모델 서빙(sklearn_logistic_regression 활용) 테스트
- train.py로  모델 만들기
    - ```python sklearn_logistic_regression/train.py --no-conda```
- 모델 API 서빙
    - API 올릴때
        - (base) root@xx:/mlflow/examples/sklearn_logistic_regression# mlflow models serve -m runs:/365c19766cd9481c85f2fd218691d803/model --env-manager local --port 5001
    - 호출할때
        - 참고 : https://mlflow.org/docs/latest/models.html#deploy-mlflow-models
        ```
        curl http://127.0.0.1:5001/invocations -H 'Content-Type: application/json' -d '{
        "dataframe_split": {"columns":["x"], "data":[[1], [-1]]}
        }'
        ```
  
### MLFLOW+GridSearchCV
- GridSearchCV를 돌면 parent run(최적)과 child runs(각각 파라미터로 만든거)로 구분된다. 이걸 잘 이용하면 파라미터 최적화를 이용해서 활용할수 있을듯
- 참고 : https://dailyheumsi.tistory.com/258
  
### MLFlow Tracking 튜토리얼
- 같은 실험인데 그룹별로 나눠서 할때, 활용하면 좋을 자료
- 참고 : https://pizzathief.oopy.io/mlflow-tracking-tutorial#8454a753-ca84-4759-9190-c32cbd1d8d35

