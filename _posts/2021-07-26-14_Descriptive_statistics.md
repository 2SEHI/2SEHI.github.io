---
title:  "기술통계량"
excerpt: ""

toc: true
toc_sticky: true
 
date: 2021-07-26
last_modified_at: 2021-08-28
use_math: true
comments: true
---

[참고 사이트]: http://www.incodom.kr/%EA%B8%B0%EC%88%A0%ED%86%B5%EA%B3%84%ED%95%99#h_a73a285dd3eb66fe16f55f14870d82d2



## 기술통계학(Descriptive statistics)이란 

통계학은 크게 **기술(Descriptive) 통계**와 **추리(inferential) 통계** 두 부분으로 나누어집니다.

### 기술 통계

- 관측을 통해 얻은 데이터에서 그 데이터의 특징을 규명하기 위한 통계적 기법입니다.
- 데이터 분석 전에 전체적인 데이터의 분포를 이해하고 통계적 수치를 제공하는 것을 목적으로 자료를 요약하기 위한 기초적인 통계량을 의미합니다. 이를 EDA (Exploratory Data Analysis) 탐색적 데이터 분석 이라고 부릅니다.

### 추리 통계

- 수집된 데이터를 기반으로 모집단의 특성을 추론하고 예측하는 데 사용하는 통계적 기법입니다.
  다시 말해, 기술 통계학은 측정이나 실험에서 수집한 데이터의 정리, 표현, 요약, 해석 등을 통해 데이터의 특성을 규명하는 통계적 분야인데 주로 수집된 데이터의 평균이나 분산 등의 통계량이나 도표를 통해 데이터의 특징을 파악합니다. 예시로는 1인당 국민소득, 전국수학능력평가시험 성적과 백분위 등이 있습니다.

<br>

### 기술통계의 종류

#### [ 1. 중심경향성(central tendency) ](#중심경향성)
- 수집한 데이터를 **대표하는 값**이 무엇인지 혹은 어떤 값에 **집중**되어 있는지를 나타냅니다.
  
  

- ex) 평균(mean), 중앙값(median), 최빈값(mode) 등이 있습니다.



#### [2. 분산도(variation) ](#분산도)

- 우리가 수집한 데이터가 **어떻게 퍼져 있는지**를 나타내는 것으로 변산성(variability)라고도 합니다.
- 중심경향성이 자료가 **무엇을 중심으로** 모여있는가를 나타내는 것이라면, 변산성 측정치는, 그 **모여있는 정도**를 의미합니다.
- ex) 범위(range), 표준편차(standard deviation), 사분위수(quantile)



#### [3. 분포(distribution)](#분포)

- 변인의 전체 모양을 살펴 데이터가 **정상분포 곡선에서 얼마나 벗어나는지**를 나타낸다.
  ex) 왜도(데이터의 분포가 좌우로 치우친 정도), 첨도(데이터의 분포가 위아래로 치우친 정도)



#### [4. 빈도(frequency)와 백분율(percent)]#빈도와-백분율)

- 각 값에 속한 사례의 수와 전체 사례 중 **해당 값이 차지하는 비율**을 나타낸다.
  ex) 빈도, 빈도분포, 백분위



#### [5. 표준오차(standard error)](#표준오차)

- 여러 표본의 평균값의 표준 편차.




---

### 중심경향성

- 자료의 중심을 나타내는 숫자로, 자료를 전체를 나타냅니다.
- 평균, 중위수, 최빈값 등이 있습니다.
- 비율 척도라고도 부릅니다.
- 비율 척도를 대상으로 직접 빈도 분석을 수행한 결과는 큰 의미가 없습니다.



#### 평균(mean)

- 데이터들의 합을 표본의 크기로 나눈 것으로 **산술평균**이라고도 합니다.
- $평균=\frac{데이터의 합}{데이터의 개수} = \frac{\sum x_{i}}{n}$
- 이상치(일반적인 데이터를 뛰어넘는 outlier)에 약하다는 특징이 있습니다.

```
x = [100, 100, 200, 400, 500]

```



#### 중위수(median)

- 데이터를 작은 값에서 큰 값으로 정렬하여 중간에 위치한 값을 의미합니다. 중앙값, 중간값이라고도 합니다.
  - 데이터의 개수가 홀수라면 중앙값은 정렬된 결과의 가운데 수입니다
  - 데이터의 개수가 짝수라면 중앙값은 가운데 두 수의 평균입니다.
- 자료를 크기 순으로 정렬할 수만 있으면 되므로 서열척도/등간척도/비율척도에서 쓸 수 있으며 명명척도에서는 쓸 수 없다고 합니다.





#### 최빈값(mode)

- 최빈값은 데이터 집합에서 가장 많이 등장한 데이터입니다.
- 모든 척도에 가능하나 주로 범주변수(명명척도, 서열척도)에서 사용한다고 합니다.





---

### 분산도
- 우리가 수집한 데이터가 **어떻게 퍼져 있는지**를 나타내는 것으로 변산성(variability)라고도 합니다.
- 중심경향성이 자료가 **무엇을 중심으로** 모여있는가를 나타내는 것이라면, 변산성 측정치는, 그 **모여있는 정도**를 의미합니다.
- ex) 범위(range), 표준편차(standard deviation), 사분위수(quantile) 등이 있습니다.
- 평균으로부터 각 변량이 떨어진 거리들에 대한 평균.편차(deviation)를 제곱한 후, 이를 합산해 데이터의 총 개수(n) 또는 (n-1)로 나눈 통계량



#### 범위(range)
- 최댓값과 최솟값 간의 차이로, 자료가 최대 최소 어느정도까지 퍼져있는가를 나타냅니다.
- $범위=최댓값-최솟값$




#### 분산(variance)

- 평균에서 데이터가 벗어난 정도를 수치화한 값입니다.
$s^{2}=\frac{\sum (x_{i}-\bar{x})^{2}}{n-1}$
- 분산이 클수록 데이터가 평균에서 많이 벗어나 있다는 뜻이며, 
  분산이 작을 수록 데이터가 평균 주변에 모여 있습니다.
- 분산은 제곱값이기 때문에  항상 0이상의  양수입니다.
- [n이 아닌 n-1로 나누는 이유](https://m.blog.naver.com/sw4r/221021838997)



#### 표준편차

- 평균에서 데이터가 벗어난 정도를 수치화한 값입니다.
- **분산의 양의 제곱근**으로 표준편차가 작으면 데이터가 평균에 몰려있다는 것을 의미합니다.



#### 사분위수

- 75번째 백분위수와 25번째 백분위수 사이의 차이로 IQR이라고도 합니다.
  - 백분위소 : 어떤 값들의 퍼센트가 이 값 혹은 더 작은 값을 갖고  (100-p) 퍼센트가 이 값 혹은 더 큰 값을 갖도록 하는 값으로 분위수 라고도 합니다.



#### 최댓값(maximum) 

- 변량 중 가장 큰 값



#### 최솟값(minimum) 

- 변량들 중 가장 작은 값



---

### 분포
- 변량의 퍼져 있는 정도를 설명하는 기술통계량으로 분포가 일정하고 대칭형이고 첨도와 왜도 통계량이 모두 0인 것을 정규분포의 형태라고 말합니다.



#### 왜도
- 데이터 분포의 기울어짐 즉 비대칭성을 나타내는 통계량
- 절대값이 클수록 기울기가 커지고, 분포 꼬리가 길어지며, 통계량 기호가 +이면 데이터 분포가 오른쪽으로, -이면 왼쪽으로 꼬리가 길어집니다.



#### 첨도
- 데이터 분포의 뾰족한 정도를 설명하는 통계량
- 첨도 통계량이 0보다 크면 정규분포보다 뽀족하고 0보다 작으면 정규분포보다 평평한 분포입니다.



---

### 빈도와 백분율

#### 빈도
- 데이터의 출현 횟수를 뜻합니다.



#### 백분위

- 크기가 있는 값들로 이뤄진 자료를 순서대로 나열했을 때 백분율로 나타낸 특정 위치의 값입니다. 일반적으로 크기가 작은 것부터 나열하여 가장 작은 것을 0, 가장 큰 것을 100으로 합니다.

---


### 표준오차

- 표본 평균 분포의 표준편차 . 표본평균과 모평균 간의 표준 간격을 측정 한것으로 표본 평균을 이용하여 모평균을 구할 때 얼마나 큰 오차가 생길지 알 수 있습니다. 모집단으로부터 수많은 표본들을 추출한 후, 각 표본들에 대해 평균을 구합니다. 그리고 각 평균들에 대한 전체 평균을 다시 구하고, 각 평균들이 전체 평균으로부터 평균적으로 얼마나 떨어져 있는지를 나타냅니다.