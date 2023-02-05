---
layout: post
title:  "[시계열] 시계열분석 간단 정리"
subtitle:   "Statistics"
categories: data
tags: stat
comments: true
---

오랜만에 시계열분석 내가 알아보기 좋게 정리

---

[참고 블로그](https://5ohyun.tistory.com/65)에 정리가 너무 잘되어있어서 보고 내 식대로 정리
  
### Lecture 0. 데이터 분석 사이클
- 0) 문제정의 : 무엇을 분석할지 정함
- 1) 데이터 수집 :
- 2) 데이터 전처리 : 기초통계 및 Transform 등
- 3) 데이터 정리 : 데이터 한곳에 담기(Data Warehouse), 바꾸기 및 정리(Data Mart) + 분리(Data Split)
    - DataMart 목적 : 웨어하우스를 변경하지 않고 복사하여 조금 더 목적에 맞게 전처리
- 4) 데이터분석 : 기초통계 + 모델링 + 검증 + 에러분석(얼마나향상될지[참고](http://taewan.kim/tutorial_manual/ml_yearning/030.basic_error_analysis/14/))
- 5) 결과정리 : 시각화 + 의사결정 + 지식화 + 공유
  
### Lecture 1. 시계열 데이터 기초
- 횡단면 데이터 : 특점시점+다수독립변수
- 시계열 데이터 : 다수시점+특정독립변수
- 시계열 횡단면 데이터 : 다수시점+다수독립변수
- 패널 데이터 : 다수시점+다수독립변수(독일변수 및 시점), 연구자 가장 선호
  
### Lecture 2. 시계열 알고리즘
- 알고리즘 선택 방법
    - 1) 문제가 어디에 속하는지 -> 분석기획(가설/방향)
    - 2) 알고리즘마다 입력은 무엇인지 -> 데이터전처리(준비)
    - 3) 알고리즘마다 출력은 무엇인지 -> 결과해석 가능
- 주로 사용되는 알고리즘
    - 1) Regression
    - 2) Regularization
    - 3) Clustering
- 대표적 성분
    - 1) 빈도 : 일,주,월,년 사람이 정해야함
    - 2) 추세
    - 3) 계절성
    - 4) 주기
    - 5) 시계열 분해 : 추세/계절성/잔차
    - 6) 더미변수
    - 7) 지연값 : Lag, ARIMA 활용
  
### Lecture 3. 시계열 데이터 패턴 추출
- 파이썬으로 성분 패턴 추출
  
### Lecture 4. 시계열 데이터 분리 및 회귀분석
- 검증지표 : R^2, t검정, F검정, AIC/BIC
- 잔차진단
    - 회귀분석시 잔차진단 가정 : 잔차분포가 정규성, 독립성, 등분산성
    - 시계열 회귀 잔차진단 : 정상성(백색잡음), 정규분포, 자기상관(시간흐름에서 독립), 등분산성
- 회귀분석 : t검정, skew, kurtosis 체크
  
### Lecture 5. 시계열 데이터 시각화
- X와 Y의 관계를 보기위해 특성파악
- 알고리즘의 결과를 냉정하게 판단하기 위해
  
### Lecture 6. 분석성능 확인
- 검증지표 : R^2, MAE 등
- 편향-분산 상층관계 : [사진 참고](https://bkshin.tistory.com/entry/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-12-%ED%8E%B8%ED%96%A5Bias%EC%99%80-%EB%B6%84%EC%82%B0Variance-Trade-off)
    - Underfitting : Bias 증가, Variance 감소 -> 모델 복잡도 낮음
    - Overfitting : Bias 감소, Variance 증가 -> 모델 복잡도 높음
  
### Lecture 7. 잔차진단
- 백색잡음
    - 1) 잔차 ~ i.i.d : 잔차들은 정규분포이고 평균 0과 일정한 분산을 가져야함. 잔차들끼리는 독립
    - 2) 잔차들간의 상관관계
        - 잔차들이 시간의 흐름에 따라 상관성이 없어야함
        - Autocorrelation 자기상관함수 : 같은 변수, 자기 자신에 대한 상관관계
        - Partial Autocorrelation Function 편자기상관함수 : 자기상관함수에서 시간 사이의 상관성을 제거한 상관함수
- 잔차진단 방향
    - 정상성, 정규분포, 자기상관, 등분산성 확인
    - 1) 정상성 테스트 - 데이터에서 추세나 계절성 없는것
    - 2) 정규분포 테스트 - H1:정규분포가 아니다
    - 3) 자기상관 테스트 - H1:시간이 지나면 autocorrelation은 0이 아니다. 자기상관관계가 있다.
    - 4) 등분산성 테스트 - H1:시간이 지나면 등분산이 아니다. 발산하는 분산이다.
- 잔차분석 시각화하고 테스트 결과 확인
  
### Lecture 8. 시계열 데이터 전처리
- 전처리 방법
    - (1) Scaling : 상황에 맞게 스케일링 하면 됨(StandardScaler, minmaxScaler 등)
    - (2) 다중공선성 제거 : VIF, PCA를 통해 변수 선택
        - 1) X와 Y 상관관계 확인
        - 2) X 변수들간의 상관관계 확인
        - 3) Variance Inflation Factor(VIF)
            - 독립변수를 다른 독립변수들의 선형회귀로 나타내, 가장 상호의존적인 변수 제거
            - 특정 변수를 눈으로 보고 삭제 가능
            - Principal Component Analysis(PCA)로 소차원으로 바꿈, 특정변수를 제거하는 것이 아니라 눈으로 확인 힘듦
    - (3) 의존성이 높은 변수들에 패널티를 주는 정규화
  
### Lecture 9. 시계열 머신러닝 알고리즘 
- Standard Regression, Lidge Regression, Lasso Regression, Elastic Net 식, 장단점 체크
- Bagging : Bias가 낮은 모델들을 통해 Variance를 줄임
- Boosting : Variance가 낮은 모델들을 합쳐 Bias를 줄임
  
### Lecture 10. 타겟 데이터 정상성 변환
- 정상성(Stationarity) : 평균, 분산, 공분산이 시간의 흐름에 따라 변하지 않음
    - 비정상성 Y를 정상성으로 변환(필수는 아님)
    - 안정성과 예측력이 향상됨 : 모델이 단순해져 overfitting 감소
    - 로그, 차분, Box-Cox 등으로 변환
    - 잔차 정상성 변환 후, 통계량으로 확인