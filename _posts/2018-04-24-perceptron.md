---
layout: post
title:  "[케라스]_(1.1) 케라스로 퍼셉트론 프로그래밍하기"
subtitle:   "PYTHON"
categories: pro
tags: deep
comments: true
---

케라스를 활용하여 단일 레이어 퍼셉트론을 프로그래밍 해보자.

```python
import matplotlib.pyplot as plt
import numpy as np
from keras.layers import *
from keras.models import *
from keras.utils import *
from collections import Counter
```

    /Users/mac/anaconda3/lib/python3.6/site-packages/h5py/__init__.py:36: FutureWarning: Conversion of the second argument of issubdtype from `float` to `np.floating` is deprecated. In future, it will be treated as `np.float64 == np.dtype(float).type`.
      from ._conv import register_converters as _register_converters
    Using TensorFlow backend.



```python
x = np.linspace(1, 10, 1000) #x를 1에서 10까지 천등분을 한다.
y = 2* x + 1
# y를 정해주었다.
```


```python
model = Sequential()

model.add(Dense(1, activation='linear', input_shape=(1,))) 
# 하나의 레이어를 만들겠다. 하나의 아웃풋이므로 1, 하나의 피쳐므로 input_shpae에 1
model.summary() 
# 결과에서 param는 웨이트와 바이어스가 있으므로 2개

model.compile(loss='mse', optimizer='sgd')

model.fit(x,y, epochs=20)
# 에폭을 20을 주니 백 프로파게이션을 계속하면서 loss가 줄어든다.
```


```python
pred_x = []
pred_x = np.append(pred_x, 12)
pred_x = np.append(pred_x, 14)

# pred_x에 12, 14를 넣는다.

pred_y = model.predict(pred_x)
print(pred_x)
print(pred_y)
```

    [12. 14.]
    [[25.059443]
     [29.084412]]



```python
plt.plot(x,y)
plt.scatter(pred_x,pred_y)
plt.show()
# x,y에 대해 플랏을 그려보고 에측치를 스캐터로 찍어보았다.
```


![png](/Users/mac/statssy_folder/statssy.github.io/assets/img/post_img/output_4_0.png)



```python
print(model.get_weights())
# 실제 웨이트와 바이어스를 볼 수 있다.
```

    [array([[2.0124848]], dtype=float32), array([0.90962404], dtype=float32)]

참고 : [Keras를 활용한 Deep Learning 입문](https://www.udemy.com/keras-deep-learning/learn/v4/overview)