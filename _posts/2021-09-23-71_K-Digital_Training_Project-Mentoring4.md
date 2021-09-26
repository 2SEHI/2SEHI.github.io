---
title: "[전력계량기OCR인식] 2021/9/23 멘토링 4회차 - 객체탐지2"

date: 2021-09-23
last_modified_at: 2021-09-23

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 전력계량기 OCR인식

---

## 1.Client쪽에서 기계학습

# 멘토링 내용


## 1.계량기 영역 도출
다음과 같이 노란색 박스가 그려진 네 개 사각형 부분을 기준으로 matchTemplate하여 계량기 영역을 검출해보는 것을 조언받았습니다. 또는 전력량 세그먼트 부분을 추출하는 것에 대해서는 위에 움푹 패인 부분도 비슷한 비율로 객체영역이 잡히니까 모델훈련을 해야 한다고 말씀해주셨습니다.

그래서 matchTemplate도 하고 모델 훈련도 하는 것보다 처음부터 selectiveSearch 로 문자영역부분에 가중치를 주어서 이미지를 모델훈련하는게 좋다고 생각했습니다.

<img src="\assets\images\71_K-Digital_Training_Project-Mentoring4_1.png" alt="질문 이미지" style="zoom:67%;" />



## 2.기울기 기준

계량기 영역을 도출하고 나면 투시변환을 하고 기울기를 조절해야 하는데 카메라의 왜곡에 의해 이미지 영역마다 기울기의 정도가 다르므로 계량기 영역을 대략 3등분으로 나누고 컨투어링한 문자영역의 대각선각도를 이용하여 각 부분의 기울기를 다르게 조정하라고 말씀해주셨습니다.

<img src="\assets\images\71_K-Digital_Training_Project-Mentoring4_1.png" alt="질문 이미지" style="zoom:67%;" />



# 오늘 해본 것

1. 일단 전력량계량기 이미지에서 많이 오염된 이미지는 훈련 성능에 영향을 주므로 별도로 분리했습니다. 모뎀 이미지도 매칭되므로 제거한 전력량계량기 이미지 의 파일명과 같은 모뎀 이미지도 별도로 분리했습니다. 사용가능한 전력량계량기 이미지는 879장입니다.
2. selectiveSearch 와 VGG모델을 이용한 비행기 영역 추출 예시에 대해 공부해보았습니다. 전력량계량기도 selectRIO로 좌표에 대한 정보를 파일에 저장하고 모델을 훈련시키면 자동으로 전력량계량기 영역을 추출할 수 있을 것 같습니다.

- 참고사이트 : [https://towardsdatascience.com/step-by-step-r-cnn-implementation-from-scratch-in-python-e97101ccde55](https://towardsdatascience.com/step-by-step-r-cnn-implementation-from-scratch-in-python-e97101ccde55)

- 소스코드 : [https://github.com/2SEHI/OCR-Text-Detection/blob/main/RCNN-airplane.ipynb](https://github.com/2SEHI/OCR-Text-Detection/blob/main/RCNN-airplane.ipynb)
