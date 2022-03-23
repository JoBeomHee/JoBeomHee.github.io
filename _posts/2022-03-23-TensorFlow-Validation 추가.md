---
title: Validation 추가
author: BeomBeomJoJo
date: 2022-03-23 11:33:00 +0800
categories: [Tensorflow]
tags: [Tensorflow]
math: true
mermaid: true
---

## **Validation 값 추가**

## **소개**
지난번 앞에서 모델을 훈련 시킬 때 fit 함수를 이용하여 검증값을 test로 하였습니다. <br/>
그러나 엄밀히 따지면, 훈련셋에 검증값이 들어가고 그 검증값으로 다시 테스트를 진행한다는 것은 **평가에 검증값이 반영되는 문제가 있습니다.** <br/>
때문에 훈련셋, 테스트셋, 검증셋은 엄밀히 분리가 되는 것이 좋은 형태입니다.

<br/>

## **Validation 데이터 구조**
* 일반적으로 Train 데이터의 일부를 잘라서 Validation 데이터로 사용하는 것이 좋습니다.
* 전체 데이터를 다음 표와 같은 구조로 배치합니다.

|데이터|데이터|
|-------|-------|
|x_train|y_train|
|x_validation|y_validation|
|x_test|y_test|

<br/>

## **개발 환경**
* **TensorFlow Version : 2.8.0**
* **Keras Version : 2.8.0**
* **Python Version : 3.10.3**
* **Developer Tools : Visual Studio Code**

<br/>

## **예제 코드**
* 1에서 10까지의 데이터를 훈련시킬 때 101에서 105의 데이터를 검증용 데이터로 사용하였습니다.
* 이후 11에서 20까지의 데이터로 테스트 진행합니다.

```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt

from tensorflow import keras

# 학습데이터 준비
x_train = np.array([1,2,3,4,5,6,7,8,9,10])
y_train = np.array([1,2,3,4,5,6,7,8,9,10])
x_test = np.array([11,12,13,14,15,16,17,18,19,20])
y_test = np.array([11,12,13,14,15,16,17,18,19,20])
x_val = np.array([101,102,103,104,105])
y_val = np.array([101,102,103,104,105])

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
history = model.fit(x_train, y_train, epochs = 1000, batch_size = 1,
                    validation_data=(x_val, y_val))

# 평가 예측
mse = model.evaluate(x_test, y_test, batch_size=1)
print(f"mse : {mse}")

y_predict = model.predict(x_test)
print(y_test)

# 결과를 그래프로 시각화
plt.scatter(x_test, y_test, label='y_true')
plt.scatter(x_test, model.predict(x_test), label='y_pred')
plt.legend()
plt.show()
```

## **실행 결과**
* 실행 결과, mse 수치는 매우 낮게 나왔고, predict 값도 매우 정확하게 나왔습니다.

```
...
...
...
Epoch 994/1000
10/10 [==============================] - 0s 5ms/step - loss: 7.8252e-07 - mse: 7.8252e-07 - binary_crossentropy: -68.6215 - val_loss: 0.2428 - val_mse: 0.2428 - val_binary_crossentropy: -1555.4222
Epoch 995/1000
10/10 [==============================] - 0s 6ms/step - loss: 4.7069e-07 - mse: 4.7069e-07 - binary_crossentropy: -68.6215 - val_loss: 0.2400 - val_mse: 0.2400 - val_binary_crossentropy: -1555.4222
Epoch 996/1000
10/10 [==============================] - 0s 5ms/step - loss: 6.3412e-07 - mse: 6.3412e-07 - binary_crossentropy: -68.6215 - val_loss: 0.2308 - val_mse: 0.2308 - val_binary_crossentropy: -1555.4222
Epoch 997/1000
10/10 [==============================] - 0s 5ms/step - loss: 1.3460e-06 - mse: 1.3460e-06 - binary_crossentropy: -68.6215 - val_loss: 0.2259 - val_mse: 0.2259 - val_binary_crossentropy: -1555.4222
Epoch 998/1000
10/10 [==============================] - 0s 6ms/step - loss: 2.2757e-06 - mse: 2.2757e-06 - binary_crossentropy: -68.6215 - val_loss: 0.2316 - val_mse: 0.2316 - val_binary_crossentropy: -1555.4222
Epoch 999/1000
10/10 [==============================] - 0s 5ms/step - loss: 1.1131e-06 - mse: 1.1131e-06 - binary_crossentropy: -68.6215 - val_loss: 0.2350 - val_mse: 0.2350 - val_binary_crossentropy: -1555.4222
Epoch 1000/1000
10/10 [==============================] - 0s 5ms/step - loss: 9.9507e-07 - mse: 9.9507e-07 - binary_crossentropy: -68.6215 - val_loss: 0.2330 - val_mse: 0.2330 - val_binary_crossentropy: -1555.4222
10/10 [==============================] - 0s 1ms/step - loss: 3.7291e-04 - mse: 3.7291e-04 - binary_crossentropy: -221.1140
mse : [0.0003729141899384558, 0.0003729141899384558, -221.1139678955078]
[11 12 13 14 15 16 17 18 19 20]
```

![결과값](https://user-images.githubusercontent.com/22911504/159686036-95645e27-2a1d-4da2-aad2-add4cfef520e.png)
