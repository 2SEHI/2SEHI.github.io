---
title:  "[디스플레이센서 이상요인 분석] 2021/8/10 멘토링 5회차 - RandomForest"
excerpt: "분류모델 훈련 및 검증"

date: 2021-08-10
last_modified_at: 2021-08-12

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 디스플레이센서 이상요인 분석
---



## 오늘 한 것 - 소스파일 정리 후 재실행하여 결과 넣기



### 레이블 분류 라벨링 

1. 값이 상위 5%이내를 불량품으로 지정

   

2. 값이 상위 1%이내를 불량품으로 지정

<div style="text-align:center"><img src="\assets\images\32_K-Digital_Training_Project-Mentoring5_1.png" alt="32_K-Digital_Training_Project-Mentoring5_1" style="zoom:50%;" /><img src="\assets\images\32_K-Digital_Training_Project-Mentoring5_2.png" alt="32_K-Digital_Training_Project-Mentoring5_2" style="zoom: 50%;" /></div>

### PCA 

- 57개의 피처가 나머지 피처에 대해 90% 설명가능

<div style="text-align:center"><img src="\assets\images\32_K-Digital_Training_Project-Mentoring5_5.png" alt="32_K-Digital_Training_Project-Mentoring5_5" style="zoom:48%;" /></div>

### 분류 모델 훈련

- **RandomForestClassifier**

  - 재현율이 0.2정도로 매우 낮으며 피처중요도가 가장 높은 피처와 레이블의 분포를 그려보니 그래도 -4이하로는 레이블값이 양품(0)으로 나옵니다. => 조금은 분류가 되는 거 같음

  

<div style="text-align:center"><img src="\assets\images\32_K-Digital_Training_Project-Mentoring5_6.png" alt="32_K-Digital_Training_Project-Mentoring5_6" style="zoom:60%;" /></div>


  - GridSearchCV로 하이퍼파라미터 튜닝을 해봤지만 0.3정도까지밖에 올라가지 않습니다.

  - 오버샘플링을 해봤는데 재현율이 0.6정도로 올라갑니다

  - 하지만, 피처중요도가 가장 높은 피처와 레이블의 분포도 그래프를 그려보니 피처의 값과 상관없이 레이블이 0과 1의 값을 갖고 있어 데이터가 혼잡해 보입니다.

    

- SVM

- XGBClassifier



### 재현율을 높일 수 있는 다른 방안

1. 임계값 조정 : Binarizer객체에 임계값을 설정하여 fit과 transform하고 재현율이 높은 지점 찾기
2. 하이퍼 파라미터 조정
3. RFE로 성능이 좋아지는 지점의 피처추출해보기

