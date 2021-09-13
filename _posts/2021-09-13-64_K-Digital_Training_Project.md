---
title:  "[전력계량기OCR인식] 2021/9/13 멘토링 3회차 - 객체탐지"

date: 2021-09-13
last_modified_at: 2021-09-13

use_math: true
comments: true

categories:
  - K-Digital Project

---


# 이번주에 해볼 것



#### 1. cv2.matchTemplate()

- 샘플이미지로 해봄. 성능 향상을 위해선 더 많은 계량기 ROI 이미지를 이용해야 함
- 회전, 크기 변화 시 적용 어려움
- 계량기 타입별로 별도의 훈련이 필요함



#### 2. cv2.CascadeClassifier()

- 시도해볼만함
- 성능 확인해보고 이용 여부 판단해 볼 예정
- 이용 시 계량기 ROI 영역을 이용하여 분류기 훈련 필요



#### 3. Selective Search

- window sliding과 달리 이미지 픽셀의 컬러, 무늬, 크기, 형태에 따라 유사한 region을 계층적 그룹핑하여 계산하는 방식임

- 모든 계량기 이미지에 대해 일괄된 전처리를 수행하여 일정한 특징을 검출할 수 있는지 확인 필요함

- 현재까지는 이미지 파일의 색상 공간을 hsv로 읽고 saturation으로 배열 변환했을 때, 

  액정 영역에서 특정 값이 나타날 수 있는지 확인하는 중