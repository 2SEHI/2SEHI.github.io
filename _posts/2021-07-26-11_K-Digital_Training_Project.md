---

title:  "[디스플레이센서 이상요인 분석] 2021/7/26 멘토링 1회차 내용"
excerpt: "멘토링 1회차를 진행했습니다. 프로젝트에 대한 구체적인 내용과 다음 멘토링까지의 과제에 대한 설명이 있었습니다."

date: 2021-07-26
last_modified_at: 2021-08-07

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 디스플레이센서 이상요인 분석
---



## 공정이상요인분석

#### 1. 멘토링 시간 

- 2021/07/26 18:20 ~ 18:50

#### 2. 이미 만들어진 프로젝트 기반으로 하거나 처음부터 직접 다 만들 것인가를 결정해야됨.

#### 3. 프로젝트 기간 

- 7/26부터 5주간 진행

#### 4. 어떤 데이터인가?

  -  디스플레이 생산라인에 있는 센서 데이터

#### 5. 이 데이터로 무엇을 할 것인가?

- 불량품에 영향을 주는 요인(= 센서 5개를 추출)을 찾을 것임.

#### 6. 컬럼에 대한 정보

- 컬럼의 개수는 841개
- 모든 컬럼의 데이터는 숫자
- 각 컬럼이 갖는 의미는 알지 못함.

#### 7. 7/29 목요일까지 할 것

- 데이터 탐색하기 - 거의 집계하는 수준이 될것임
  - 기술 통계량자료를 수집 : 열의 개수, 분산, 범위 등 구하기
  - 요인 분석방법에 대해서 **학습**하기
    - 주성분 분석
    - 상관계수 파악
    - 다중공선성 해결 방법 - VIF(분산 팽창 요인)
    - 분류는 feature_importances_ , 회귀는 cost_function  으로 주요요인을 찾을 수 있음. 이번 프로젝트는 변수가 너무 많아 어렵다.

오늘은 기술 통계량 자료와 요인 분석방법에 대해 공부를 하고, 내일 팀원들과 이야기를 나눠보기로 했습니다.

---



<br>

그리고 아래는 우리 팀에서 진행할 프로젝트는 아니지만 프로젝트 내용을 적어봤다.



## X-ray

- X-ray손 이미지로 뼈의 나이를 추정 
- Deep Learning의 OpenCV로 해야됨.

- 이미지파일과 엑셀파일을보면 각 파일들이 몇살 몇개월의 뼈나이가 적혀있음. 이 뼈나이는 의사2명이 뼈사진을 보고 나이를 추측한 것임.
- 성장도표 책자에서는 아이의 키를 참고하면 됨. 자세히 살펴보기엔 시간이 없으므로 그정도만 참고할 것.
- 골연령 인식을 할 때, 손에서 뼈나이를 예측하는 방법에 대해 조사해 봐야 함.
  - 손목과 손가락관절로 예측하는 방법을 많이 사용함.
  - 단 학습시 손 이미지를 다 넣어서 학습하는게 아니라 손목이나 손가락 일부에 대한 bbox만들어서  학습을 해야함. 하지만 데이터가 너무 많아지므로 bbox를 그리는 자동화를 만들어야 함. 
  - RoI를 어떻게 추출할까를 고민해야됨.  
  - 골연령에 대한 CNN모델이 있는데 찾아보면 인터넷에 나올 것임. 
- PYQT를 이용하여 이미지 파일을 불러오면  뼈나이를 예측하는 윈도우 프로그램을 만들어야 함.
