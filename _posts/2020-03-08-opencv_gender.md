---
layout: post
title:  "[이미지 인식]주피터 노트북으로 시각화한 OpenCV를 활용한 성별 및 나이 판별하기 "
subtitle:   "DeepLearning"
categories: data
tags: deep
comments: true
---


참고 사이트에서 본 코드와 내가 주피터 노트북(jupyter notebook)에서 시각화를 할 수 있게 조금 바꿔보았다.

참고 : [Interesting Python Project of Gender and Age Detection with OpenCV](https://data-flair.training/blogs/python-project-gender-age-detection/?fbclid=IwAR1d43CEOg-5dnzxPTxE06uB60leOzBKe30bRZewVvsM88V7v6TmTN9GDYI)
  
---
# 주피터 노트북으로 시각화한 OpenCV를 활용한 성별 및 나이 판별하기 (파이썬) 
  
---

## 성별 

## 프로젝트 개요
### CV(Computer Vision) 이란
- 디지털 이미지나 비디오를 사람처럼 컴퓨터가 보고 인식할수 있게 하는 연구 분야

### Open CV
- Open Source Computer Vision의 약자
- 이 라이브러리는 실시간 이미지 및 비디오를 처리하는 동시에 분석 기능을 제공한다.
- 딥러닝 프레임워크인 TensorFlow, Caffe 및 PyTorch를 지원


### CNN 이란
- Convolutional Neural Network는 주로 이미지 인식이나 처리, NLP를 목적으로 하는 Deep Neural Network(DNN)
- 알다시피 CNN은 입력, 출력, 히든 레이어가 있고 이번 케이스에서는 Regularized Multilayer Perceptron이다.

### 성별과 나이 판별의 목적
- 성별과 나이 판별기를 만들어서 추측

### 이 프로젝트에 대해
- 성별과 나이는 ranges- (0 – 2), (4 – 6), (8 – 12), (15 – 20), (25 – 32), (38 – 43), (48 – 53), (60 – 100) (8 nodes in the final softmax layer)로 하였음
- 아무래도 메이크업이나 빛이나 방해요소 및 표정 등 때문에 정확하기는 힘들다.

### CNN 아키텍쳐
- 여기서는 3가지의 Convolutional Layer가 있다
    - Convolutional layer; 96 nodes, kernel size 7
    - Convolutional layer; 256 nodes, kernel size 5
    - Convolutional layer; 384 nodes, kernel size 3
- 2개의 full connected layer와 512노드 마지막은 출력 레이어는 소프트맥스로 하였다.

이 프로젝트는 이 4가지 문제를 풀 것이다.
- 얼굴 감지(Detect faces)
- 성별 분류(Classify into Male/Female)
- 8가지 연령 구간 분류(Classify into one of the 8 age ranges)
- 결과 이미지 보여주기(Put the results on the image and display it)

### 데이터셋

- [캐글 Adience 데이터](https://www.kaggle.com/ttungl/adience-benchmark-gender-and-age-classification) 을 사용
- 26,580장의 사진과 2,284명의 피사체, 8개의 나이 구간으로 1GB의 사이즈 이다.
  
  
## 프로젝트 전개

1. 파일 다운로드 : [모델 다운로드](https://drive.google.com/file/d/1yy_poZSFAPKi0y2e2yj9XDe1N8xXYuKB/view)
  
  - opencv_face_detector.pbtxt : 얼굴을 감지 하기 위한 파일. 텍스트 포맷. 텐서플로우 파일
  - opencv_face_detector_uint8.pb : 얼굴을 감지 하기 위한 파일. 프로토콜 버퍼 파일. 모델의 훈련된 가중치와 그래프 definition. 이진 포맷
  - age_deploy.prototxt : 연령에 대한 네트워크 구성 설정
  - age_net.caffemodel : 연령에 대한 레이어 매개 변수의 내부 상태를 정의
  - gender_deploy.prototxt :성별에 대한 네트워크 구성
  - gender_net.caffemodel : 성별에 대한 레이어 매개 변수의 내부 상태를 정의
  - a few pictures to try the project on
    
2. argparse 라이브러리로 argument parser를 만든다.

3. 얼굴 나이 성별의 프로토콜과 모델을 초기화 한다.

4. 모델의 평균값과 나이 리스트, 성별 리스트를 분류해서 만들어 놓는다.

5. readNet()을 사용해서 네트워크를 로드한다. 첫번째 파라미터는 트레인된 웨이트고 두번째는 구성 상태이다.

6. 비디오 스트리밍을 캡쳐하고 패딩은 20

7. 비디오면 스트리밍 읽고 저장, 비디오가 아니면 break

8. highlightFace() 함수를 DNN Face Detector 모델로 만든다.(x1,y1,x2,y2 구함)

9. 4개의차원의 스케일과 리사이징 한다

10. 성별을 고른다.

11. 나이를 구한다.

12. imshow()로 보여준다. (그러나 나는 쥬피터 노트북으로 했기 때문에 다른 방법으로 함) [참고](https://zzsza.github.io/data/2018/01/23/opencv-1/)



## 주피터 노트북 코드


gap.py의 맨 마지막 부분만
```python
cv2.imshow("Detecting age and gender", resultImg)
```
->
```python
from matplotlib import pyplot as plt
b, g, r = cv2.split(resultImg)
image2 = cv2.merge([r, g, b])
plt.imshow(image2)
plt.xticks([])  # x축 눈금
plt.yticks([])  # y축 눈금
plt.show()
```
로 변경 하였다.

```python
!pip install matplotlib 
```
   

```python
!pip install open-python
```


```python
%run gad.py --image woman1.jpg
```

    Gender: Female
    Age: 38-43 years
    


![png](/assets/img/post_img/2020-03-08-opencv_gender_img/output_2_1.png)

