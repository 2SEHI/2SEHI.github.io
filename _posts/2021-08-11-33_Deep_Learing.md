---

title:  "[DeepLearning] 딥러닝의 특징"
excerpt: "케라스 창시자에게 배우는 딥러닝"

date: 2021-08-11
last_modified_at: 2021-08-29

use_math: true
comments: true
categories:
  - Deep Learning
---

*케라스 창시자에게 배우는 딥러닝을 읽고 작성된 포스팅입니다.*



## 딥러닝의 특징

- 머신 러닝은 처리가 용이하도록 사람이 초기 입력 데이터를 여러 방식으로 변환해야 하는데 이를 특성 공학(Feature Engineering)이라고 합니다. 
- 딥러닝은 입력 데이터의 변환 단계를 완전히 자동화하는데 Representation Learning이라고 합니다.



### 딥러닝의 "기본" 순서

1. 데이터 분리

```python
from kears.datasets import mnist
(train_images, train_labels), (test_images, test_labels) = mnist.load_data()
```



2. 신경망 생성

- Dense 는 조밀하게 연결된 완전 연결 신경망 층입니다.

```python
from keras import models
from keras import layers

network = models.Sequential()
network.add(layers.Dense(512, activation='relu', input_shape=(28 * 28, )))
network.add(layers.Dense(10, activation='softmax'))
```



3. 컴파일 단계

신경망이 훈련 준비를 마치기 위한 단계로 손실함수, 옵티마이저, 성능 지표를 설정합니다.

- 손실함수(loss functions) : 훈련 데이터에서 신경망의 성능을 측정하는 방법으로 네트워크가 옳은 방향으로 학습될 수 있도록 도와줍니다.

- 옵티마이저(optimizer) : 입력된 데이터와 손실 함수를 기반으로 네트워크를 업데이트하는 메커니즘입니다.
- 성능 지표 : 훈련과 테스트 과정을 모니터링할 지표(정확도, 정밀도 등..)를 설정합니다

```python
network.compile(optimizer='rmsprop', loss='categorical_crossentropy',
				metrics=['accuracy'])
```



4. 훈련 이미지 데이터 조정

- 0과 1사이로 스케일을 조정합니다
- 훈련이미지 reshape합니다
  - (60000, 28, 28) --> (60000, 28 * 28)

```python
train_images = train_images.reshape((60000, 28 * 28))
train_images = train_images.reshape('float32') / 255

test_images = test_images.reshape((10000, 28 * 28))
test_images = test_images.reshape('float32') / 255
```



5. 레이블 준비

- 레이블 범주형 인코딩
  - 1차원 정수배열을 (입력 받은 훈련 데이터의 개수, 클래스 개수)크기의 2차원 배열로 변경합니다

```
from keras.utils import to_categorical

train_labels = to_categorical(train_labels)
test_labels = to_categorical(test_labels)
```



6. 모델 학습

- fit메소드로 모델을 훈련 시킵니다
- 훈련데이터에 대한 손실값과 정확도(accuracy)가 출력됩니다.

```python
network.fit(train_images, train_labels, epochs=5, batch_size=128)
```



7. 테스트 성능 확인

```python
test_loss, test_acc = network.evaluate(test_images,test_labels)
print('test_acc : ', test_acc)
```



# 신경망을 위한 데이터 표현



## 텐서 

- 다차원 넘파치 배열에 데이터를 저장하는 기본 데이터 구조입니다



## 스칼라

- 하나의 숫자만 담고 있는 텐서입니다.
- 0차원 텐서 또는 0D 텐서라고 부릅니다.
- 스칼라 축의 개수는 0입니다
- 축 개수를 랭크라고도 합니다.



## 벡터

- 1D 텐서로 1개의 축을 가집니다


## 행렬

- 백터의 배열을 뜻합니다
- 2D텐서이며 행과 열이라는 2개의 축을 가집니다




---
에포크(epoch)
- 전체 훈련 데이터에 수행되는 각 반복횟수

# 손실함수(Loss Function)
- 훈련 데이터에서 신경망의 성능을 측정하는 방법으로 네트워크가 옳은 방향으로 학습될 수 있도록 도와줍니다


## cross entropy error(CEE : 교차 엔트로피)
- 실제 분포 q에 대해서 알지 못하는 상태에서 모델링을 통하여 구한 분포 p를 통해 q를 예측함
- 실제값과 예측값이 맞는 경우 0으로 수렴
- 값이 틀릴 경우에는 값이 커짐


## binary crossentropy
- 2개의 클래스가 있는 분류에서 사용


## categorical crossentropy
- 여러개 클래스있는 분류
- 레이블이 인코딩되어 있을 것이라 기대



## mean absolute error(MAE : 평균 절대 오차)
- 예측과 타깃 사이 거리의 절댓값. 
- 회귀문제에서 사용


## mean squared error(MSE : 평균 제곱 오차)
- 예측과 타깃 사이 거리의 제곱
- 회귀문제에서 사용


## sparse categorical crossentropy
- 정수 레이블에 사용


 시그모이드 함수
 - 입력된 데이터에 대해서 0과 1사이의 값을 출력하여 해당 값이 둘 중 하나에 속할 확률로 해석할 수 있도록 만들어줍니다.


 소프트맥스 회귀
 - 하나의 샘플 데이터에 대한 예측값으로 모든 가능한 정답지에 대해서 정답일 확률의 합이 1이 되도록 
 - 다중 클래스 분류에서 사용함.