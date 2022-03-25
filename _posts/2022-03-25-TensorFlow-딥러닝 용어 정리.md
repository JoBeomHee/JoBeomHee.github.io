---
title: Deep Learning 소개 및 용어 정리
author: BeomBeomJoJo
date: 2022-03-25 12:33:00 +0800
categories: [딥러닝]
tags: [딥러닝]
math: true
mermaid: true
---

## **참조**
* https://davinci-ai.tistory.com/20

<br/>

## **딥러닝(Deep Learning)이란**
* 딥러닝은 머신러닝의 한 분야입니다.
* 연속된 층에서 점진적으로 학습을 하는 것에 강점이 있으며, 기계 학습의 새로운 방식입니다.
* 딥러닝에서 딥은 연속된 층으로 학습한다는 의미입니다.
* 층의 숫자는 모델의 깊이를 나타내기 때문입니다.

<br/>

## **신경망(Neural Network)**
* 딥러닝은 기본 층들을 쌓아서 구성한 신경망이라는 모델을 사용하여 학습을 진행합니다.
* 신경망은 뉴런(Neuron)들로 이루어진 그룹을 의미합니다.
* 신경망은 원래 신경 생물학의 용어입니다.
* 뉴런들의 끝이 다른 뉴런들과 연결된 구조입니다.
* 뇌 구조를 이해하는 것에서 영감을 받아서 딥러닝 모델의 핵심 개념을 설명하지만, 실제로 뇌를 모델링하여 만든 것은 아닙니다.
* 그저 하자의 데이터 학습을 새로운 방식으로 하는 수학 모델이라고 보시면 됩니다.

<br/>

### Biological Neural Netowork(BNN)
* 생물학적으로는 Biological Neural Network(BNN) 이라고 합니다.
* 생물학적인 뉴런은 시냅스를 통해서 전기 신호를 이용하여 의사소통 합니다.
* 시냅스는 세포의 끝에 위치하여 다음 세포로 신호를 전달하는 역할을 합니다.

![1](https://user-images.githubusercontent.com/22911504/160100126-2008a9c0-1aa5-4076-b82d-9d8003c6601c.png)

<br/>

### **Artifical Neural Network(ANN)**
* 생물학적인 뉴런의 개념을 따서 만든 컴퓨팅적인 모델입니다.
* 두개의 층(Layers) 이상으로 구성되어 있다는 것이 특징입니다.

<br/>

### **뉴런(Neuron)**
* 생물학적인 뉴런의 개념에 기초한 수학적인 함수를 의미합니다.
* 뉴런이 활성 중인지에 따라서 활성함수가 결정됩니다.
* 해당 뉴런의 결과가 0이라면, 신호를 주고 받지 않는 비활성화된 뉴런이란 것을 알 수 있습니다.

![2](https://user-images.githubusercontent.com/22911504/160100130-4776c2ef-b802-47ae-8cda-7f3820740afb.png)

<br/>

### **신경망의 구성**
* Input Layer(입력층)
  * 입력 뉴런들로 구성된 층을 의미합니다.
* Output Layer(출력층)
  * 결과물을 생산하는 출력 뉴런을 의미합니다.
* Hidden Layer(은닉층)
  * 입력층과 출력층 사이에 있는 Layer를 의미합니다.
* Deep Neural Network(DNN)
  * 적어도 3개의 층 이상으로 이루어진 신경망을 의미합니다.(2개 이상의 Hidden Layer)

![3](https://user-images.githubusercontent.com/22911504/160100131-d9e0c52a-3f03-4ef2-844e-aa676d4d4327.png)

<br/>

### **신경망 학습이란?**
* 신경망 학습의 목표는 가중치의 정확한 값을 찾는 것에 있습ㄴ디ㅏ.
* 어떤 층이 있을 때, 해당 층을 데이터가 거치면서 일어나느 변환은 해당 층의 가중치를 매개변수로 가지는 함수를 통해서 일어납니다.
* 학습은 주어진 입력을 정확한 출력으로 매핑시키기 위해 모든 층의 가중치를 찾는 것을 의미합니다.
* 신경항 학습의 순서는 다음과 같습니다.
  * 데이터를 입력받습니다.
  * 층에 도달할 때마다 해당 층의 가중치를 사용하여 출력 값을 계산합니다.
  * 모든 층에서 반복합니다. 마지막으로는 예측 값이 나옵니다.
  * 예측 값은 실제 값과 비교하는 손실 함수를 통해서 점수를 계산합니다.
  * 해당 점수를 피드백으로 삼아서, 손실 점수가 감소되는 방향으로 가중치를 수정합니다.(Optimizer)
  * 위의 과정을 반복합니다.
  * 손실 함수를 통해 계산되는 손실 점수를 최소화시키는 가중치를 찾아냅니다.

<br/>

## **신경망 학습과 관련된 용어**
### **Weight(가중치)**
* 뉴런 사이의 연결 강도를 의미합니다.
* 신경망이 훈련을 하는 동안, 업데이트되어 weight가 변경됩니다.
* Learnable Parameters 라고도 부릅니다.

![4](https://user-images.githubusercontent.com/22911504/160100132-b11e4e11-05a5-4e45-87ff-dc22256af82c.png)

<br/>

### **Bias(편향)**
* Bias 뉴런의 가중치를 의미합니다.
* 해당 값 또한, 훈련 시 업데이트 됩니다.

![5](https://user-images.githubusercontent.com/22911504/160100137-acc1d7c6-e582-434b-976a-c013a0012591.png)

<br/>

### **Activation Function(활성함수)**
* 주어진 입력값들을 받은 뉴런의 출력값을 돌려주는 함수입니다.
* 현재 뉴런의 활성화에 따라 출력값을 결정합니다.
  * 1은 뉴런을 활성화, 0은 뉴런을 비활성화
* 대표적인 활성함수
  * **Sigmoid Function**
    * 초창기에 많이 사용된 활성함수로, S자형 곡선을 가지는 함수입니다.
    * AND, OR, XOR 연산을 다루는데 적합한 함수입니다.
    * 또한 Binary Classification에 적합한 함수입니다.
    * 출력 값의 범위 : [0, 1]

![6](https://user-images.githubusercontent.com/22911504/160100139-78e708e2-80fb-4642-816e-623d5a8d5b36.png)

```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-x))
```

  * **Hyperbolic Tangent**
    * tanh함수는 sigmoid의 s자 곡선 형태를 가지면서, 출력값의 범위는 -1과 1 사이인 함수입니다.
    * 값의 범위 : [-1, 1]
  * **ReLU (Rectified Linear Unit)**
    * Sigmoid의 대안으로 나온 활성함수로, 0 이하의 수는 0을, 양수인 경우에는 양수를 그래도 반환하여 값의 왜곡이 적어지는 함수입니다.
    * 값의 범위 : [0, int]

![7](https://user-images.githubusercontent.com/22911504/160100141-f5a8f494-c54f-4120-a547-bf8c58c3f37b.png)

```python
import numpy as np

def relu(x):
    return np.maximum(0, x)
```