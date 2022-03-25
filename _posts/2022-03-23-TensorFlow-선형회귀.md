---
title: 1에서 100까지 값 예측하기
author: BeomBeomJoJo
date: 2022-03-23 11:33:00 +0800
categories: [Tensorflow]
tags: [Tensorflow]
math: true
mermaid: true
---

## **소개**
안녕하세요. TensorFlow & Keras 를 이용하여 간다한 선형회귀 모델을 작성 후 <br/>
1에서 100까지 값 즉, y=ax+b 의 1차 함수를 예측하는 파이썬 코드를 작성해 보았습니다. <br/>

<br/>

## **개발 환경**
* **TensorFlow Version : 2.8.0**
* **Keras Version : 2.8.0**
* **Python Version : 3.10.3**
* **Developer Tools : Visual Studio Code**

<br/>

## **예제 코드**
* 예제 코드는 아래와 같습니다.

```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt

from tensorflow import keras

# 학습데이터 준비
x = np.array([1,2,3,4,5,6,7,8,9,10])
y = np.array([1,2,3,4,5,6,7,8,9,10])

# Keras의 Sequential모델을 선언
model = keras.Sequential([
    # 첫 번째 Layer: 데이터를 신경망에 집어넣기
    keras.layers.Dense(32, activation='relu', input_shape = (1, )),
    # 두번째 층 
    keras.layers.Dense(32, activation='relu'),
    # 세번째 출력층: 예측 값 출력하기
    keras.layers.Dense(1)
])

# 모델을 학습시킬 최적화 방법, loss 계산 방법, 평가 방법 설정
model.compile(optimizer='adam',loss='mse',metrics=['mse', 'binary_crossentropy'])
# 모델 학습
history = model.fit(x,y, epochs = 500, batch_size = 100)

print(model.predict([3]))
print(model.predict([5]))

# 결과를 그래프로 시각화
plt.scatter(x, y, label='y_true')
plt.scatter(x, model.predict(x), label='y_pred')
plt.legend()
plt.show()
```

## **실행 결과**
```
...
...
...
1/1 [==============================] - 0s 3ms/step - loss: 0.0131 - mse: 0.0131 - binary_crossentropy: -68.6216
Epoch 496/500
1/1 [==============================] - 0s 3ms/step - loss: 0.0130 - mse: 0.0130 - binary_crossentropy: -68.6216
Epoch 497/500
1/1 [==============================] - 0s 2ms/step - loss: 0.0130 - mse: 0.0130 - binary_crossentropy: -68.6216
Epoch 498/500
1/1 [==============================] - 0s 4ms/step - loss: 0.0129 - mse: 0.0129 - binary_crossentropy: -68.6216
Epoch 499/500
1/1 [==============================] - 0s 3ms/step - loss: 0.0128 - mse: 0.0128 - binary_crossentropy: -68.6216
Epoch 500/500
1/1 [==============================] - 0s 3ms/step - loss: 0.0128 - mse: 0.0128 - binary_crossentropy: -68.6216
[[3.1414802]]
[[5.073011]]
```
* 위와 같이 Epoch 을 500번 주어, 학습을 시켰습니다.
* 시각 자료로 확인 결과 **학습, 검증** 2개의 데이터 값이 거의 유사한 것을 확인할 수 있습니다.

![결과값](https://user-images.githubusercontent.com/22911504/159668477-37de1db2-7f84-4d18-ac52-3648ed989aa9.png)
