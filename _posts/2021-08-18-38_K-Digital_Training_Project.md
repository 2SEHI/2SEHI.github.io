---

title:  "[디스플레이센서 이상요인 분석] 2021/8/18 전처리과정 정리"
excerpt: "지금까지 수행한 전처리과정에 대해 정리해보았습니다"

date: 2021-08-18
last_modified_at: 2021-08-18

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 디스플레이센서 이상요인 분석
---



## 오늘 한 것

PPT정리를 위해 지금까지 수행했던 내용을 큰 흐름만 정리해보았습니다.

### 1. 컬럼제거

<div style="text-align:center"><img src="\assets\images\38_K-Digital_Training_Project.png" alt="38_K-Digital_Training_Project" style="zoom:100%;" /></div>

### 2. PCA주성분 분석

- 누적 기여율
- Elbow Point



### 3. 레이블링

- 레이블의 값이 0.01이상인 것과 아닌 것
- 상위 5%이상인 것과 아닌 것
- 상위 10%이상인 것과 아닌 것



### 4. 분류 모델 수행

#### 4.1 모델 종류

- SVM
- RandomForest
- XGBoost



#### 4.2 트리구조 확인



#### 4.3 RFE수행

- PCA 누적 기여율이 90%인 n_component개수만큼 줄임



#### 4.4 성능 지표

- 재현율로 확인



#### 
