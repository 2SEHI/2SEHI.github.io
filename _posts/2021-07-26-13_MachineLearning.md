---
title:  "[데이터 전처리]3.수치형 데이터 처리"
excerpt: "수치형 데이터의 단위환산와 스케일링에 대해 알아봅니다."


toc: true
toc_sticky: true
 
date: 2021-07-26
last_modified_at: 2021-07-26
use_math: true
comments: true

categories:
  - Machine Learning
---



# 수치형 데이터 처리



## 1. 수치형 데이터의 단위 환산

- 하나의 데이터 셋에서 서로 다른 측정 단위를 사용하면 전체 데이터의 일관성 측면에서 문제가 발생할 수 있습니다.   
  
- 외국의 데이터셋은 국내에서 거의 사용하지 않는 도량형 단위(마일, 야드, 온스, 화씨 등)를 사용하므로 우리나라에서 사용하는 단위로 변환해야 합니다.   
- 따라서 각 단위가 국내에 맞게 어떤 계산식으로 변환해야 하는지 검색해봐야 합니다.

- 자동차 연비 mpg dataset을 이용하여 단위환산하는 방법을 알아보겠습니다.



### mpg데이터 읽어오기


```python
import pandas as pd

#auto-mpg.csv 파일의 데이터 가져오기
mpg = pd.read_csv('../data/auto-mpg.csv',header=None)
mpg.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 398 entries, 0 to 397
    Data columns (total 9 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   0       398 non-null    float64
     1   1       398 non-null    int64  
     2   2       398 non-null    float64
     3   3       398 non-null    object 
     4   4       398 non-null    float64
     5   5       398 non-null    float64
     6   6       398 non-null    int64  
     7   7       398 non-null    int64  
     8   8       398 non-null    object 
    dtypes: float64(4), int64(3), object(2)
    memory usage: 28.1+ KB



### 컬럼 이름 부여

- 각 컬럼명이 숫자이므로 컬럼명을 부여하겠습니다.


```python
#컬럼 이름 만들기
mpg.columns = ['mpg','cylinder','displacement','horsepower','weight',
              'acceleration','model year','origin','name']
mpg.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 398 entries, 0 to 397
    Data columns (total 9 columns):
     #   Column        Non-Null Count  Dtype  
    ---  ------        --------------  -----  
     0   mpg           398 non-null    float64
     1   cylinder      398 non-null    int64  
     2   displacement  398 non-null    float64
     3   horsepower    398 non-null    object 
     4   weight        398 non-null    float64
     5   acceleration  398 non-null    float64
     6   model year    398 non-null    int64  
     7   origin        398 non-null    int64  
     8   name          398 non-null    object 
    dtypes: float64(4), int64(3), object(2)
    memory usage: 28.1+ KB



## mpg를 우리나라의 단위로 변환

- MPG(Miles Per Gallon)란 차량의 연료 소비의 정도를 나타내는데 갤런 당 마일의 단위입니다.
- 마일/갤런 의 단위를 킬로/리터로 환산을 해야하는데 그 계산법은 다음과 같습니다.

    1 Mile = 1.60934 Km   
    1 Gallon = 3.78541 L

    이를 기반으로 미터법으로 환산해 보면,  1 mpg = 0.425 Km/L 라는 계산이 나옵니다.   
    
- 한국식 연료소비량을 나타내는 kpi컬럼에 mpg를 변환하여 넣어줍니다.


```python
mpg['kpi']=mpg['mpg'] * 1.60934 / 3.78541
print(mpg.head())
```

        mpg  cylinder  displacement horsepower  weight  acceleration  model year  \
    0  18.0         8         307.0      130.0  3504.0          12.0          70   
    1  15.0         8         350.0      165.0  3693.0          11.5          70   
    2  18.0         8         318.0      150.0  3436.0          11.0          70   
    3  16.0         8         304.0      150.0  3433.0          12.0          70   
    4  17.0         8         302.0      140.0  3449.0          10.5          70   
    
       origin                       name       kpi  
    0       1  chevrolet chevelle malibu  7.652571  
    1       1          buick skylark 320  6.377143  
    2       1         plymouth satellite  7.652571  
    3       1              amc rebel sst  6.802286  
    4       1                ford torino  7.227428  




**이런 식으로 온도(°C와 °F), 길이(inchi와 cm) 등과 같은 도량형 단위변환을 해주면 됩니다.**



# 2. 스케일링(Scaling)



## 표준화(Standardization)

- 표준화는 어떤 특성의 값들이 정규분포를 따른다고 가정하고 값들을 0의 평균, 1의 표준편차를 갖도록 변환해주는 것입니다.
- 쉽게 말하면 평균을 기준으로 특성의 값이 어느정도 떨어져 있는가를 나타내는 것입니다.
- 더 쉽게 말하면 10명에 대한 월소득 평균을 나타내고자 하는데 9명의 월소득은 200만원이고 1명의 월소득이 1,000만원으로 10명의 월소득 평균이 280만원이 되어버렸습니다. 이때 10명 중 9명의 월소득이 200만원인데 평균이 280만원이라고 할 수 있을까요? 이 문제를 해결하기 위해서 평균에서 크게 벗어난 데이터를 제거하여 값들이 중앙으로 모이도록 하는 것을 표준화라고 합니다.

- 표준화 과정을 거치면 0\~1또는 \-1~1사이의 값으로 많이 변경됩니다.
- 유지보수할 때 클라이언트쪽을 변화시키지 않기 위해서 사용하기도 합니다.



## 정규화(Normalization)

- 한 특성 내에 가장 큰 값은 1로, 가장 작은 값은 0으로 변환하여 데이터 분포를 조정하는 방법입니다.
- 수능 영어 점수를 90점 맞았고 토익 점수를 100점을 맞았다고 했을때 토익점수가 수능영어점수에 비해 더 높다고 할 수 있을까요? 당연히 만점(범위)의 기준이 다르므로 토익점수가 높다고 할 수 없습니다. 이를 0~1사이의 숫자로 일반화시키는 것을 정규화라고 합니다.
- min과 max의 편차가 크거나, 다른 열에 비해 데이터가 지나치게 큰 열에 사용됩니다.



## 스케일러 종류

- sklearn라이브러리에서는 다음과 같은 스케일러를 제공하고 있으므로 데이터에 따라 맞게 사용하면 됩니다.
- 주성분 분석에서 많이 사용합니다.

1. [StandardScaler](#1-standardscaler)
2. [RobustScaler](#2-robustscaler)
3. [MinMaxScaler](#3-minmaxscalerab)
4. [MaxAbsScaler](#4-maxabsscaler)
5. [Normalizer](#5-normalizer)
6. [QuantileTransformer](#6-quantiletransformer)



## scikit-learn에서 정규화순서

1. Scaler객체생성
2. fit메소드를 호출할 때 2차원ndarray을 매개변수로 넘겨준다
3. transform 메소드를 실행하여 데이터를 변환해주는데 2차원ndarray을 매개변수로 넘겨준다



### 1. StandardScaler

- 각 특성의 평균을 0, 분산을 1로 스케일링하여 데이터를 정규분포로 만듭니다. 
- 하한값과 상한값이 존재하지 않아 어떤 알고리즘에서는 문제가 있을 수 있습니다. 
- 회귀보다 분류에 유용합니다.


```python
import pandas as pd
import numpy  as np
from sklearn import preprocessing

arr = [3000000, 2500000, 2700000, 1800000, 2200000,
      3200000, 4000000, 2000000, 2300000, 10000000]
income = np.array(arr).reshape(10,1)

# 1. 표준편차 객체를 생성
scaler = preprocessing.StandardScaler()

# 2. fit : 머신러닝은 Series나 DataFrame을 사용할 수 없고 numpy의 ndarray만 사용가능
scaler.fit(income)

# 3. transform : numpy의 ndarray만 사용가능
income_scaled = scaler.transform(income)
income_scaled
```

`array([[-0.16135681],
       [-0.37940656],
       [-0.29218666],
       [-0.6846762 ],
       [-0.5102364 ],
       [-0.07413691],
       [ 0.27474268],
       [-0.5974563 ],
       [-0.46662645],
       [ 2.89133962]])`



### 2. RobustScaler

- 평균과 분산 대신 Median과 IQR(interquartile range)을 사용하며, 이상치(Outlier)의 영향을 최소화 합니다.
- 각 특성들의 중앙값을 0, IQR(75% - 25%)이 1로 스케일링합니다. 
- 데이터에 이상치가 많으면 평균과 표준편차에 영향을 주기 때문에 중앙값과 4분위를 이용합니다.
- StandardScaler와 비슷하지만, 이상치의 영향을 최소화합니다.


```python
from sklearn import preprocessing
import matplotlib.pyplot as plt

arr = [3000000, 2500000, 2700000, 1800000, 2200000,
      3200000, 4000000, 2000000, 2300000, 10000000]
income = np.array(arr).reshape(10,1)

## 캔버스 생성 사이즈 3 * 3
fig = plt.figure(figsize=(3,3)) 
 
plt.boxplot(income) ## 상자 수염 그림 출력
 
fontsize=13 ## 폰트 사이즈
plt.xticks([1],['INCOME'], fontsize=fontsize) ## x축 눈금 라벨
plt.title('Box Plot',fontsize=fontsize) ## 타이틀
plt.show()
```


​    
![png](\assets\images\13_num_preprocessing.png)
​    



```python
# 1. 표준편차 객체를 생성
scaler = preprocessing.RobustScaler()

# 2. fit : 머신러닝은 Series나 DataFrame을 사용할 수 없고 numpy의 ndarray만 사용가능
scaler.fit(income)

# 3. transform : numpy의 ndarray만 사용가능
income_scaled = scaler.transform(income)
income_scaled
```

`array([[ 0.43243243],
       [-0.10810811],
       [ 0.10810811],
       [-0.86486486],
       [-0.43243243],
       [ 0.64864865],
       [ 1.51351351],
       [-0.64864865],
       [-0.32432432],
       [ 8.        ]])`



### 3. MinMaxScaler(a,b)

- 각 특성의 하한값을 a, 상한값을 b로 스케일링합니다. 
- a=0, b=1일 경우 Normalization으로 표기할 때도 있습니다.
- 분류보다 회귀에 유용합니다.


```python
from sklearn import preprocessing
arr = [3000000, 2500000, 2700000, 1800000, 2200000,
      3200000, 4000000, 2000000, 2300000, 10000000]
income = np.array(arr).reshape(10,1)

# 1. 표준편차 객체를 생성
scaler = preprocessing.MinMaxScaler(feature_range=(2, 3))

# 2. fit : 머신러닝은 Series나 DataFrame을 사용할 수 없고 numpy의 ndarray만 사용가능
scaler.fit(income)

# 3. transform : numpy의 ndarray만 사용가능
income_scaled = scaler.transform(income)
income_scaled
```

`array([[2.14634146],
       [2.08536585],
       [2.1097561 ],
       [2.        ],
       [2.04878049],
       [2.17073171],
       [2.26829268],
       [2.02439024],
       [2.06097561],
       [3.        ]])`



### 4. MaxAbsScaler

- 각 특성을 절대값이 0과 1사이가 되도록 스케일링합니다.
- 즉, 모든 값은 -1과 1사이로 표현되며, 데이터가 양수일 경우 MinMaxScaler와 같습니다.


```python
from sklearn import preprocessing
arr = [3000000, 2500000, 2700000, 1800000, 2200000,
      3200000, 4000000, 2000000, 2300000, 10000000]
income = np.array(arr).reshape(10,1)

# 1. 표준편차 객체를 생성
scaler = preprocessing.MaxAbsScaler()

# 2. fit : 머신러닝은 Series나 DataFrame을 사용할 수 없고 numpy의 ndarray만 사용가능
scaler.fit(income)

# 3. transform : numpy의 ndarray만 사용가능
income_scaled = scaler.transform(income)
income_scaled
```

`array([[0.3 ],       [0.25],       [0.27],       [0.18],       [0.22],       [0.32],       [0.4 ],       [0.2 ],       [0.23],       [1.  ]])`



### 5. Normalizer

- 앞의 4가지 스케일러는 각 특성(열)의 통계치를 이용하여 진행됩니다. 
- 그러나 Normalizer의 경우 각 샘플(행)마다 적용되는 방식입니다. 
- 이는 한 행의 모든 특성들 사이의 유클리드 거리(L2 norm)가 1이 되도록 스케일링합니다. 
- 일반적인 데이터 전처리의 상황에서 사용되는 것이 아니라, 모델(특히나 딥러닝) 내 학습 벡터에 적용하며, 특히나 피쳐들이 다른 단위(키, 나이, 소득 등)라면 더더욱 사용하지 않습니다.


```python
from sklearn import preprocessing
arr = [3000000, 2500000, 2700000, 1800000, 2200000,      3200000, 4000000, 2000000, 2300000, 10000000]
income = np.array(arr).reshape(10,1)# 1. 표준편차 객체를 생성
scaler = preprocessing.Normalizer()# 2. fit : 머신러닝은 Series나 DataFrame을 사용할 수 없고 numpy의 ndarray만 사용가능
scaler.fit(income)# 3. transform : numpy의 ndarray만 사용가능
income_scaled = scaler.transform(income)
income_scaled
```

`array([[1.],       [1.],       [1.],       [1.],       [1.],       [1.],       [1.],       [1.],       [1.],       [1.]])`



### 6. QuantileTransformer

- 데이터를 1000개의 분위로 나누어서 0과1사이에 고르게 분포를 하는 방법으로 이상치에 덜 민감합니다.


```python
from sklearn import preprocessing
arr = [3000000, 2500000, 2700000, 1800000, 2200000,      3200000, 4000000, 2000000, 2300000, 10000000]
income = np.array(arr).reshape(10,1)# 1. 표준편차 객체를 생성
scaler = preprocessing.QuantileTransformer()# 2. fit : 머신러닝은 Series나 DataFrame을 사용할 수 없고 numpy의 ndarray만 사용가능
scaler.fit(income)# 3. transform : numpy의 ndarray만 사용가능
income_scaled = scaler.transform(income)
income_scaled
```

`array([[0.66666667],       [0.44444444],       [0.55555556],       [0.        ],       [0.22222222],       [0.77777778],       [0.88888889],       [0.11111111],       [0.33333333],       [1.        ]])`



