---
title: MNIST 모델별 Image Resize 하기
author: BeomBeomJoJo
date: 2022-03-24 11:33:00 +0800
categories: [Tensorflow]
tags: [Tensorflow]
math: true
mermaid: true
---

## **소개**
안녕하세요. <br/>
오늘은 TensorFlow & Keras 에서 MNIST 모델을 Load 하여 네트워크 별로 이미지 Resize 하는 방법에 대해서 알려 드리려고 합니다. <br/>
이미지 Resize 가 필요한 이유는, 모델별로 Input Shape이 모두 다르기 때문입니다. <br/>
MNIST 데이터셋은 60,000장의 이미지가 있고, 28x28 사이즈를 가진 이미지 입니다. <br/>
하지만, Keras 에서 지원하는 기본 모델들 예를 들어, xception, inception_v1, vgg_19 등등 많은 모델들이 존재합니다. <br/>
그리고 해당 모델들은 모두 Network Size가 다릅니다. <br/>
xception 모델 같은 경우는 299x299 이고, vgg_19 모델은 224x224 처럼 모두 사이즈가 다릅니다. <br>
때문에 MNIST DataSet을 가지고 각 모델별로 Train 혹은 Inference 할 시 이미지 Resize 하는 작업이 필요합니다. <br/>
공부하면서 Image Resize 함수를 작성하였고, 기록으로 남깁니다. <br/>

<br/>

## **Image Resize 메서드**
* 아래 코드는 StackOverFlow 를 통해서 참조하여 작성한 내용입니다.
* 해당 resize 메서드에 변경 대상인 data와 input_shape 정보를 매개변수로 받습니다.
* `data = data[:50]` 은 이미지 개수가 많은 경우, **OOM Error** 즉 메모리 에러가 발생하여 임의로 이미지 개수를 제한하였습니다.
* `expand_dims` 메서드를 통해 차원을 추가하였습니다.
* 마지막으로 변경할 Input_shape 을 resize 메서드에 넣어주면, 본인이 변경하고자 하는 Input Shape으로 이미지가 resize 됩니다.

```python
def resize(data, input_shape):
    data = data[:50]
    # expand new axis, channel axis 
    data = np.expand_dims(data, axis=-1)
    # [optional]: we may need 3 channel (instead of 1)
    data = np.repeat(data, input_shape[2], axis=-1)
    # it's always better to normalize 
    data = data.astype('float32') / 255
    # resize the input shape , i.e. old shape: 28, new shape: input_shape1, input_shape2
    data = tf.image.resize(data, [input_shape[0],input_shape[1]])
    return data
```
