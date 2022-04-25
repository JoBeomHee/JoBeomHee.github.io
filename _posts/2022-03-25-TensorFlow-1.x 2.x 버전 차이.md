---
title: TensorFlow 2.x과 1.x의 차이점
author: BeomBeomJoJo
date: 2022-03-25 12:33:00 +0800
categories: [TensorFlow]
tags: [TensorFlow]
math: true
mermaid: true
---

{% include adsense.html %}

## **참조**
* [참고 사이트](https://www.tensorflow.org/guide/migrate?hl=ko#20%EC%97%90_%EB%A7%9E%EB%8F%84%EB%A1%9D_%EC%BD%94%EB%93%9C_%EC%88%98%EC%A0%95%ED%95%98%EA%B8%B0)
* [참고 사이트](https://dk-kang.tistory.com/entry/Tensorflow-1%EB%B2%84%EC%A0%84-vs-Tensorflow-2%EB%B2%84%EC%A0%84)
* [참고 사이트](http://mcheleplant.blogspot.com/2019/03/tensorflow.html)

<br/>

## **목적**
* TensorFlow 1.x 에서 2.x 로 업데이트 되면서 무엇이 바뀌었는지 확인합니다.

<br/>

## **1. Session 사용의 불필요**
* TensorFlow 1과 TensorFlow 2의 가장 큰 차이는 Session 사용의 유무 입니다.
* TensorFlow 1 같은 경우에는 원하는 값을 출력하기 위해서 Session을 열고 찾으려는 값을 그 Session을 통해서 값을 받아오는 복잡한 구조였습니다.
* 하지만, TensorFlow 2 부터는 별도의 Session 실행 없이 바로 특정 값을 계산할 수 있게 되었습니다.
> Session은 Graph를 돌리는 단위로 file과 같다고 생각하면 쉽습니다. 파일에 그래프를 넣고 저장하고 실행시킬 수 있습니다. 보다 자세한 내용은  http://mcheleplant.blogspot.com/2019/03/tensorflow.html 참고하면 됩니다.

<br/>

### **1.1. TensorFlow 1 버전**

```python
a = tf.constant(1)
b = tf.constant(2)
c = a * b
sess = tf.Session()
sess.run(c)
```

<br/>

### **1.2. TensorFlow 2 버전**

```python
a = tf.constant(1)
b = tf.constant(2)
c = a * b
print(c)
```

<br/>

## **2. 선언 최소화 및 내장 함수 사용**
* TensorFlow 1 의 경우 많은 부분이 전역적인 형태로 처리되고 있었습니다.
* 예를 들어 우리가 선언한 전역 변수나 placeholder를 사용하기 위해서는, global_variables_initializer API를 호출해야 설정한 값들을 사용할 수 있었습니다.
* 하지만, placeholder와 더불어 전역 형태로 호출하는 부분들을 생략할 수 있게 되었습니다.
* 그래서 전체적으로 코드가 간결해지고 디버깅도 쉽게 할 수 있게 되었습니다.
* 또한 TensorFlow 1에서도 Keras를 사용할 수 있었지만, TensorFlow 2 부터는 공식 소스로 편입 되면서 쉬운 Keras 코딩을 더 활발하게 이용할 수 있다는 장점이 있습니다.

<br/>

### **2.1. TensorFlow 1 버전**

```python
import tensorflow.compat.v1 as tf
 
in_a = tf.placeholder(dtype=tf.float32, shape=(2))
 
def model(x) :
    with tf.variable_scope("matmul") :
        W = tf.get_variable("W", initializer=tf.ones(shape=(2, 2)))
        b = tf.get_variable("b", initializer=tf.zeros(shape=(2)))
        return x * W + b
 
out_a = model(in_a)
 
with tf.Session() as sess :
    sess.run(tf.global_variables_initializer())
    outs = sess.run([out_a], feed_dict={in_a: [1,0] })
```

<br/>

### **2.2. TensorFlow 2 버전**

```python
import tensorflow as tf
 
W = tf.Variable(tf.ones(shape=(2,2)), name="W")
b = tf.Variable(tf.zeros(shape=(2)), name="b")
 
@tf.function
def model(x) :
    return W * x + b
out_a = model([1,0])
 
print(out_a)
```

<br/>

## **3. 속도의 차이**
* Session을 통해 호출하는 것이 동일 코드라고 하면 더 빠릅니다.
* 이유는 TensorFlow 1 에서 Session을 실행하기 전에 모든 변수 하나씩 다 선언하고 변수를 받는 곳에는 미리 placeholder라는 틀을 만들고 모든 것을 설정한 후에 진행하였기 때문입니다.
* 쉽게 달리기로 예로 들어 봅니다. TensorFlow 1 같은 경우에는 달리기 시합 전에 가벼운 소재 유니폼을 입고, 러닝화를 신고, 준비 운동까지 끝 맞춘 상황에서 달리기를 하는 것입니다.
* 그래서 달리기를 할 때, 최적의 속도로 달릴 수 있습니다.
* 그러나 TensorFlow 2는 달리기 시합 전에 최소한의 장비만 착용하고 달리는 것입니다.
* 때문에 속도는 TensorFlow 2 보다 TensroFlow 1이 더 빠릅니다.

<br/>

## **4. Function 사용**
* TensorFlow 2의 속도가 느려진 것을 보완하기 위해 나온 것이, function 화 입니다.

```python
@tf.function
def add(a, b):
    return a + b
```
* 위와 같이 우리가 만든 임의의 function에 tf.function을 달아주면, 컴파일 되어 해당 함수는 미리 세이브드 모델에 저장됩니다.
* 그래서 필요에 따라서 우리가 만든 코드에 저장된 함수를 빠르게 부를 수 있고, 불러진 함수는 보다 빠르게 처리해줌으로써 속도를 높여주고 있습니다.

<br/>

{% include adsense.html %}