---
title:  "다중공선성과 VIF"
excerpt: "다중공선성과 VIF는 무엇이고 어떤 처리를 해야 할지 알아보았습니다"

toc: true
toc_sticky: true
 
date: 2021-07-28
last_modified_at: 2021-07-28

use_math: true
comments: true

categories:
  - Machine Learning
---



## 다중 공선성(Multicollnearity)
- 회귀분석은 독립변수들로 변수들을 선정해 주어야 하는데 변수들 간의 상관관계가 높아지면 회귀계수 추정치의 신뢰성과 안전성에 문제를 발생시킵니다. 이 때 다중회귀모형에서 일부 독립변수(예측변수, 회귀자; regressor)가 다른 독립변수와 높은 상관관계를 가지는 것을 다중 공선성이라고 합니다.
- **이 때 주의할 점**은 **상관관계분석은 변수들 간의 관련성을 나타내는 것**이므로, 인과관계로 파악해서는 안됩니다.(쉽게 생각하면 상관관계는 방향성이 없고, 인과관계는 원인과 결과의 방향성을 가집니다). 다중공선성이 있다면 상관관계가 높지만 상관관계가 높다고 다중공선성이 반드시 있는 것은 아닙니다. 





## 다중공선성 확인 : VIF(Variance Inflation Factor)
- VIF란 분산 팽창 인수라고 하며, 이 값은 다중회귀분석에서 독립변수가 다중 공선성(Multicollnearity)의 문제를 갖고 있는지 판단하는 기준이 됩니다. 
- 판단 기준은 주로 10보다 크면 그 독립변수는 다중공산성일 가능성이 있다고 말하며 , 5로 두는 경우도 있습니다.





## 다중공선성 해결 방법
- 다중공선성을 가진 두 변수 중 하나를 제거
- 제거시  $R^{2}$이 유지되는 변수를 제거
- **주의할 점은 다중공선성이 높은 컬럼을 무조건 제거하는게 아닙니다. 자세한 설명은 아래 소스코드를 보며 설명하겠습니다.**





## VIF 함수
- ```statsmodels.stats.outliers_influence```의 ```variance_inflation_factor```함수를 이용합니다.



---

### 프로야구 선수 연봉 데이터셋으로 VIF확인
- 데이터는 아래 링크에서 받을 수 있습니다.
- [http://www.statiz.co.kr]( http://www.statiz.co.kr )


```python
import pandas as pd 
picher_file_path = '/content/drive/MyDrive/Colab Notebooks/data/picher_stats_2017.csv'
batter_file_path = '/content/drive/MyDrive/Colab Notebooks/data/batter_stats_2017.csv'
picher = pd.read_csv(picher_file_path)
batter = pd.read_csv(batter_file_path)
```



### 데이터 확인


```python
print(picher.head())
print(picher.columns)
print(picher.shape)
```

       선수명   팀명   승   패  세  홀드  ...  RA9-WAR   FIP  kFIP   WAR  연봉(2018)  연봉(2017)
    0   켈리   SK  16   7  0   0  ...     6.91  3.69  3.44  6.62    140000     85000
    1   소사   LG  11  11  1   0  ...     6.80  3.52  3.41  6.08    120000     50000
    2  양현종  KIA  20   6  0   0  ...     6.54  3.94  3.82  5.64    230000    150000
    3  차우찬   LG  10   7  0   0  ...     6.11  4.20  4.03  4.63    100000    100000
    4  레일리   롯데  13   7  0   0  ...     6.13  4.36  4.31  4.38    111000     85000
    
    [5 rows x 22 columns]
    Index(['선수명', '팀명', '승', '패', '세', '홀드', '블론', '경기', '선발', '이닝', '삼진/9',
           '볼넷/9', '홈런/9', 'BABIP', 'LOB%', 'ERA', 'RA9-WAR', 'FIP', 'kFIP', 'WAR',
           '연봉(2018)', '연봉(2017)'],
          dtype='object')
    (152, 22)



### 2018년 연봉 분포를 출력합니다.


```python
picher['연봉(2018)'].hist(bins=100) 
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f594f75e1d0>




![png](\assets\images\15_Multicollnearity_And_VIF.png)
    

### standard scaling을 수행합니다.


```python
picher_features_df = picher[['승', '패', '세', '홀드', '블론', '경기', '선발', '이닝', '삼진/9',
       '볼넷/9', '홈런/9', 'BABIP', 'LOB%', 'ERA', 'RA9-WAR', 'FIP', 'kFIP', 'WAR',
       '연봉(2018)', '연봉(2017)']]

# pandas 형태로 정의된 데이터를 출력할 때, scientific-notation이 아닌 float 모양으로 출력되게 해줍니다.
pd.options.mode.chained_assignment = None

# 피처 각각에 대한 standard_scaling을 수행하는 함수
def standard_scaling(df, scale_columns):
    for col in scale_columns:
        series_mean = df[col].mean()
        series_std = df[col].std()
        df[col] = df[col].apply(lambda x: (x-series_mean)/series_std)
    return df

# 피처 각각에 대한 scaling을 수행합니다.
scale_columns = ['승', '패', '세', '홀드', '블론', '경기', '선발', '이닝', '삼진/9',
       '볼넷/9', '홈런/9', 'BABIP', 'LOB%', 'ERA', 'RA9-WAR', 'FIP', 'kFIP', 'WAR', '연봉(2017)']
picher_df = standard_scaling(picher, scale_columns)

picher_df = picher_df.rename(columns={'연봉(2018)': 'y'})
picher_df.head(5)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>선수명</th>
      <th>팀명</th>
      <th>승</th>
      <th>패</th>
      <th>세</th>
      <th>홀드</th>
      <th>블론</th>
      <th>경기</th>
      <th>선발</th>
      <th>이닝</th>
      <th>삼진/9</th>
      <th>볼넷/9</th>
      <th>홈런/9</th>
      <th>BABIP</th>
      <th>LOB%</th>
      <th>ERA</th>
      <th>RA9-WAR</th>
      <th>FIP</th>
      <th>kFIP</th>
      <th>WAR</th>
      <th>y</th>
      <th>연봉(2017)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>켈리</td>
      <td>SK</td>
      <td>3.313623</td>
      <td>1.227145</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.059433</td>
      <td>2.452068</td>
      <td>2.645175</td>
      <td>0.672099</td>
      <td>-0.869000</td>
      <td>-0.442382</td>
      <td>0.016783</td>
      <td>0.446615</td>
      <td>-0.587056</td>
      <td>3.174630</td>
      <td>-0.971030</td>
      <td>-1.058125</td>
      <td>4.503142</td>
      <td>140000</td>
      <td>2.734705</td>
    </tr>
    <tr>
      <th>1</th>
      <td>소사</td>
      <td>LG</td>
      <td>2.019505</td>
      <td>2.504721</td>
      <td>-0.098502</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.059433</td>
      <td>2.349505</td>
      <td>2.547755</td>
      <td>0.134531</td>
      <td>-0.987502</td>
      <td>-0.668521</td>
      <td>-0.241686</td>
      <td>-0.122764</td>
      <td>-0.519855</td>
      <td>3.114968</td>
      <td>-1.061888</td>
      <td>-1.073265</td>
      <td>4.094734</td>
      <td>120000</td>
      <td>1.337303</td>
    </tr>
    <tr>
      <th>2</th>
      <td>양현종</td>
      <td>KIA</td>
      <td>4.348918</td>
      <td>0.907751</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.111056</td>
      <td>2.554632</td>
      <td>2.706808</td>
      <td>0.109775</td>
      <td>-0.885929</td>
      <td>-0.412886</td>
      <td>-0.095595</td>
      <td>0.308584</td>
      <td>-0.625456</td>
      <td>2.973948</td>
      <td>-0.837415</td>
      <td>-0.866361</td>
      <td>3.761956</td>
      <td>230000</td>
      <td>5.329881</td>
    </tr>
    <tr>
      <th>3</th>
      <td>차우찬</td>
      <td>LG</td>
      <td>1.760682</td>
      <td>1.227145</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>-0.043811</td>
      <td>2.246942</td>
      <td>2.350927</td>
      <td>0.350266</td>
      <td>-0.945180</td>
      <td>-0.186746</td>
      <td>-0.477680</td>
      <td>0.558765</td>
      <td>-0.627856</td>
      <td>2.740722</td>
      <td>-0.698455</td>
      <td>-0.760385</td>
      <td>2.998081</td>
      <td>100000</td>
      <td>3.333592</td>
    </tr>
    <tr>
      <th>4</th>
      <td>레일리</td>
      <td>롯데</td>
      <td>2.537153</td>
      <td>1.227145</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.059433</td>
      <td>2.452068</td>
      <td>2.587518</td>
      <td>0.155751</td>
      <td>-0.877464</td>
      <td>-0.294900</td>
      <td>-0.196735</td>
      <td>0.481122</td>
      <td>-0.539055</td>
      <td>2.751570</td>
      <td>-0.612941</td>
      <td>-0.619085</td>
      <td>2.809003</td>
      <td>111000</td>
      <td>2.734705</td>
    </tr>
  </tbody>
</table>
</div>



### 팀명 피처를 one-hot encoding으로 변환합니다.


```python
team_encoding = pd.get_dummies(picher_df['팀명'])
picher_df = picher_df.drop('팀명', axis=1)
picher_df = picher_df.join(team_encoding)

picher_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>선수명</th>
      <th>승</th>
      <th>패</th>
      <th>세</th>
      <th>홀드</th>
      <th>블론</th>
      <th>경기</th>
      <th>선발</th>
      <th>이닝</th>
      <th>삼진/9</th>
      <th>볼넷/9</th>
      <th>홈런/9</th>
      <th>BABIP</th>
      <th>LOB%</th>
      <th>ERA</th>
      <th>RA9-WAR</th>
      <th>FIP</th>
      <th>kFIP</th>
      <th>WAR</th>
      <th>y</th>
      <th>연봉(2017)</th>
      <th>KIA</th>
      <th>KT</th>
      <th>LG</th>
      <th>NC</th>
      <th>SK</th>
      <th>두산</th>
      <th>롯데</th>
      <th>삼성</th>
      <th>한화</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>켈리</td>
      <td>3.313623</td>
      <td>1.227145</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.059433</td>
      <td>2.452068</td>
      <td>2.645175</td>
      <td>0.672099</td>
      <td>-0.869000</td>
      <td>-0.442382</td>
      <td>0.016783</td>
      <td>0.446615</td>
      <td>-0.587056</td>
      <td>3.174630</td>
      <td>-0.971030</td>
      <td>-1.058125</td>
      <td>4.503142</td>
      <td>140000</td>
      <td>2.734705</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>소사</td>
      <td>2.019505</td>
      <td>2.504721</td>
      <td>-0.098502</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.059433</td>
      <td>2.349505</td>
      <td>2.547755</td>
      <td>0.134531</td>
      <td>-0.987502</td>
      <td>-0.668521</td>
      <td>-0.241686</td>
      <td>-0.122764</td>
      <td>-0.519855</td>
      <td>3.114968</td>
      <td>-1.061888</td>
      <td>-1.073265</td>
      <td>4.094734</td>
      <td>120000</td>
      <td>1.337303</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>양현종</td>
      <td>4.348918</td>
      <td>0.907751</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.111056</td>
      <td>2.554632</td>
      <td>2.706808</td>
      <td>0.109775</td>
      <td>-0.885929</td>
      <td>-0.412886</td>
      <td>-0.095595</td>
      <td>0.308584</td>
      <td>-0.625456</td>
      <td>2.973948</td>
      <td>-0.837415</td>
      <td>-0.866361</td>
      <td>3.761956</td>
      <td>230000</td>
      <td>5.329881</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>차우찬</td>
      <td>1.760682</td>
      <td>1.227145</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>-0.043811</td>
      <td>2.246942</td>
      <td>2.350927</td>
      <td>0.350266</td>
      <td>-0.945180</td>
      <td>-0.186746</td>
      <td>-0.477680</td>
      <td>0.558765</td>
      <td>-0.627856</td>
      <td>2.740722</td>
      <td>-0.698455</td>
      <td>-0.760385</td>
      <td>2.998081</td>
      <td>100000</td>
      <td>3.333592</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>레일리</td>
      <td>2.537153</td>
      <td>1.227145</td>
      <td>-0.306452</td>
      <td>-0.585705</td>
      <td>-0.543592</td>
      <td>0.059433</td>
      <td>2.452068</td>
      <td>2.587518</td>
      <td>0.155751</td>
      <td>-0.877464</td>
      <td>-0.294900</td>
      <td>-0.196735</td>
      <td>0.481122</td>
      <td>-0.539055</td>
      <td>2.751570</td>
      <td>-0.612941</td>
      <td>-0.619085</td>
      <td>2.809003</td>
      <td>111000</td>
      <td>2.734705</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### 학습데이터와 테스트 데이터 분리


```python
from sklearn import linear_model
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error
from math import sqrt

# 학습 데이터와 테스트 데이터로 분리합니다.
X = picher_df[picher_df.columns.difference(['선수명', 'y'])]
y = picher_df['y']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 회귀 분석 계수를 학습합니다 (회귀 모델 학습)
lr = linear_model.LinearRegression()
model = lr.fit(X_train, y_train)

# 학습된 계수를 출력합니다.
print(lr.coef_)
```

    [ -1863.27167152   1147.15608757 -52147.32574794   5915.51391759
       2299.44885884  -1744.6150334     397.17996335   -249.60365919
      -1024.27838506    399.1396206   12274.79760529  44088.31585257
      -3602.91866901  -5319.02202278    617.34035282   4644.19380296
        879.30541662  -3936.74747195   1521.68382584 -10999.04385918
       -700.8303505    4526.7078132   21785.5776696    6965.59101874
        154.91380911   2018.54543747  -1217.59759673   9090.86143072]



### 분산 팽창 요인 계산

- 각 컬럼별로 VIF(분산팽창요인)을 계산해 보았습니다. 10이상일 경우 다중공선성일 가능성이 있다고 했는데요.VIF Factor가 10이상인 컬럼은 다음과 같습니다.
  - FIP
  - kFIP
  - 홈런/9
  - 삼진/9
  - 이닝
  - 볼넷/9
  - 선발
  - 경기
  - RA9-WAR
  - ERA
  - WAR





```python
from statsmodels.stats.outliers_influence import variance_inflation_factor
import numpy as np
#데이터 프레임 생성
vif = pd.DataFrame()
#X 가 가지고 있는 데이터들을 이용해서 분산 팽창 요인을 계산
vif["VIF Factor"] = [variance_inflation_factor(X.values, i) 
                     for i in range(X.shape[1])]
#컬럼 이름을 복사해서 생성
vif["features"] = X.columns
#소수 첫번째 짜리까지 반올림
vif.round(1)
vif.sort_values(by='VIF Factor', ascending=False)
```

    /usr/local/lib/python3.7/dist-packages/statsmodels/tools/_testing.py:19: FutureWarning: pandas.util.testing is deprecated. Use the functions in the public API at pandas.testing instead.
      import pandas.util.testing as tm





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VIF Factor</th>
      <th>features</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>14238.289281</td>
      <td>FIP</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10264.067472</td>
      <td>kFIP</td>
    </tr>
    <tr>
      <th>27</th>
      <td>425.628625</td>
      <td>홈런/9</td>
    </tr>
    <tr>
      <th>18</th>
      <td>89.484808</td>
      <td>삼진/9</td>
    </tr>
    <tr>
      <th>23</th>
      <td>63.817732</td>
      <td>이닝</td>
    </tr>
    <tr>
      <th>15</th>
      <td>57.840351</td>
      <td>볼넷/9</td>
    </tr>
    <tr>
      <th>19</th>
      <td>39.587016</td>
      <td>선발</td>
    </tr>
    <tr>
      <th>12</th>
      <td>14.620874</td>
      <td>경기</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13.551449</td>
      <td>RA9-WAR</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.633696</td>
      <td>ERA</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10.359736</td>
      <td>WAR</td>
    </tr>
    <tr>
      <th>21</th>
      <td>7.969674</td>
      <td>승</td>
    </tr>
    <tr>
      <th>24</th>
      <td>5.883327</td>
      <td>패</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4.300985</td>
      <td>LOB%</td>
    </tr>
    <tr>
      <th>26</th>
      <td>3.765144</td>
      <td>홀드</td>
    </tr>
    <tr>
      <th>0</th>
      <td>3.207869</td>
      <td>BABIP</td>
    </tr>
    <tr>
      <th>20</th>
      <td>3.120906</td>
      <td>세</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2.979844</td>
      <td>블론</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2.493525</td>
      <td>연봉(2017)</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1.227296</td>
      <td>두산</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1.226774</td>
      <td>삼성</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1.146670</td>
      <td>NC</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1.144145</td>
      <td>SK</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.132708</td>
      <td>KT</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1.124340</td>
      <td>한화</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1.120740</td>
      <td>롯데</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.113030</td>
      <td>LG</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.109791</td>
      <td>KIA</td>
    </tr>
  </tbody>
</table>
</div>



- 다중공선성은 다른 컬럼과 원인관계에 있는 것이므로, 하나의 컬럼만 제거해도 다중공선성의 문제는 해결이 됩니다. 따라서 다중공선성이 높다고 판단된 컬럼들 중 하나(FIP)와 다중공선성이 낮은 컬럼들을 가지고 다시 분산 팽창 요인을 계산하여 VIF Factor가 낮게 나오는지 확인해보고, 낮게 나오면, 그 컬럼들을 가지고 회귀모델로 분석을 수행하여 정확성이 높아지는지 확인해봐야 합니다.


###  인과관계에 있는 컬럼들에 대해 분산팽창요인을 계산할 경우
- VIF Factor가 높은 두 컬럼 'FIP'와 'kFIP' 을 같이 넣어 분산팽창요인을 계산해보면 급격히 VIF Factor가 높아집니다.


```python
X = picher_df[['FIP','kFIP', 'WAR', '볼넷/9', '삼진/9', '연봉(2017)']]
#데이터 프레임 생성
vif = pd.DataFrame()
#X 가 가지고 있는 데이터들을 이용해서 분산 팽창 요인을 계산
vif["VIF Factor"] = [variance_inflation_factor(X.values, i) 
                     for i in range(X.shape[1])]
#컬럼 이름을 복사해서 생성
vif["features"] = X.columns
#소수 첫번째 짜리까지 반올림
vif.round(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VIF Factor</th>
      <th>features</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>669.0</td>
      <td>FIP</td>
    </tr>
    <tr>
      <th>1</th>
      <td>762.8</td>
      <td>kFIP</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.1</td>
      <td>WAR</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.8</td>
      <td>볼넷/9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.8</td>
      <td>삼진/9</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.9</td>
      <td>연봉(2017)</td>
    </tr>
  </tbody>
</table>
</div>



###  인과관계에 있는 컬럼들 중 하나를 제거하여 분산팽창요인을 계산할 경우
- 반면, 'FIP'와 'kFIP' 컬럼 중에 하나를 제거하여 분산팽창요인을 계산하면, VIF Factor가 매우 안정적인 수준으로 낮아지는 것을 볼 수 있습니다. 따라서 'FIP'와 'kFIP' 컬럼 둘 중에 하나만 삭제하면 됩니다.


```python
#피처의 개수를 줄여서 다중 공선성 문제를 해결
X = picher_df[['FIP', 'WAR', '볼넷/9', '삼진/9', '연봉(2017)']]
#데이터 프레임 생성
vif = pd.DataFrame()
#X 가 가지고 있는 데이터들을 이용해서 분산 팽창 요인을 계산
vif["VIF Factor"] = [variance_inflation_factor(X.values, i) 
                     for i in range(X.shape[1])]
#컬럼 이름을 복사해서 생성
vif["features"] = X.columns
#소수 첫번째 짜리까지 반올림
vif.round(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VIF Factor</th>
      <th>features</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.2</td>
      <td>홈런/9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.1</td>
      <td>WAR</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.3</td>
      <td>볼넷/9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.1</td>
      <td>삼진/9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.9</td>
      <td>연봉(2017)</td>
    </tr>
  </tbody>
</table>
</div>



## 회귀분석모델로 학습 및 검증수행
###  인과관계에 있는 컬럼들에 대해 학습 및 검증


```python
# 피처를 재선정합니다.
X = picher_df[['FIP', 'WAR', '볼넷/9', '삼진/9', '연봉(2017)']]
y = picher_df['y']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=19)

# 모델을 학습합니다.
lr = linear_model.LinearRegression()
model = lr.fit(X_train, y_train)

# 결과를 출력합니다.
print(model.score(X_train, y_train)) # train R2 score를 출력합니다.
print(model.score(X_test, y_test)) # test R2 score를 출력합니다. 
```

    0.9150591192570362
    0.9038759653889862



```python
y_predictions = lr.predict(X_train)
print(sqrt(mean_squared_error(y_train, y_predictions))) # train RMSE score를 출력합니다.
y_predictions = lr.predict(X_test)
print(sqrt(mean_squared_error(y_test, y_predictions))) # test RMSE score를 출력합니다.
```

    7893.462873347693
    13141.866063591096



###  인과관계에 있는 컬럼들에 대해 학습 및 검증


```python
# 피처를 재선정합니다.
X = picher_df[['FIP','kFIP', 'WAR', '볼넷/9', '삼진/9', '연봉(2017)']]
y = picher_df['y']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=19)

# 모델을 학습합니다.
lr = linear_model.LinearRegression()
model = lr.fit(X_train, y_train)

# 결과를 출력합니다.
print(model.score(X_train, y_train)) # train R2 score를 출력합니다.
print(model.score(X_test, y_test)) # test R2 score를 출력합니다. 
```

    0.9152778787072587
    0.9046627899565158



```python
y_predictions = lr.predict(X_train)
print(sqrt(mean_squared_error(y_train, y_predictions))) # train RMSE score를 출력합니다.
y_predictions = lr.predict(X_test)
print(sqrt(mean_squared_error(y_test, y_predictions))) # test RMSE score를 출력합니다.
```

    7883.291782515135
    13087.969083364505



## 요약
1. VIF(분산팽창요인)으로 다중 공선성 확인
2. VIF Factor가 10 이상인 컬럼과 아닌 컬럼들을 추출하여 다시 다중 공선성 확인
3. VIF Factor가 높다면 해당 컬럼 제거하고 다시 분석팽창요인 계산하고, 
     VIF Factor가 낮으면 4로.
4. 회귀모델로 정확도 확인하여 성능확인

