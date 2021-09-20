---

title:  "[디스플레이센서 이상요인 분석] 2021/8/1 분산, 상관관계, VIF를 이용한 컬럼제거"
excerpt: "분산이 0인 컬럼제거, 높은 상관관계의 컬럼제거, VIF계수가 30이하가 될때까지 반복하며 컬럼제거했습니다."

toc: true
toc_sticky: true

date: 2021-08-01
last_modified_at: 2021-08-07

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 디스플레이센서 이상요인 분석
---

## 오늘 한 것

### 1.분산이 0인 컬럼제거



<div style="text-align:center"><img src="\assets\images\38_K-Digital_Training_Project.png" alt="38_K-Digital_Training_Project" style="zoom:100%;" /></div>

   - VarianceThreshold(threshold)는 분산을 구하고 threshold에 설정한 임계치에 수렴하는 컬럼을 제거해줍니다.
   - get_support함수로 남겨지는 컬럼 index를 반환해주는데 이를 이용하여 남겨지는 컬럼명과 삭제할 컬럼명을 출력할 수도 있습니다.

   ```python
   from sklearn.feature_selection import VarianceThreshold
   
   # 분산이 0에 수렴하는 컬럼을 제거하고 DataFrame을 반환하는 함수
   def VarianceThreshold_selector(data):
       # 임계값으로 0을 지정
       selector = VarianceThreshold(0)
       selector.fit(data)
       # 남겨지는 컬럼index 저장
       features = selector.get_support(indices = True)
       # 남겨지는 컬럼명 저장
       features = [data.iloc[:,column].name for column in features]
       # 삭제 컬럼명 출력
       deleted_columns = [column for column in data.columns if column not in features]
       print('삭제된 컬럼명의 개수 : ', len(deleted_columns))
       print('\n 삭제된 컬럼명 : ')
       for i in deleted_columns:
           print(i)
       selector = pd.DataFrame(selector.transform(data))
       selector.index = data.index
       selector.columns = features
       # DataFrame 반환
       return selector
   
   print('컬럼제거 전 : ',  fact_data.shape)
   fact_data = VarianceThreshold_selector(fact_data)
   print('컬럼제거 후 : ',  fact_data.shape)
   ```

   

   - 삭제된 컬럼명



<div style="text-align:center"><img src="\assets\images\20_K-Digital_Training_Project.png" alt="20_K-Digital_Training_Project" style="zoom: 80%;"/></div>




   - [VarianceThreshold에 대한 문서 참조1](https://runebook.dev/en/docs/scikit_learn/modules/generated/sklearn.feature_selection.variancethreshold)
   
   - [VarianceThreshold에 대한 문서 참조2](https://datascienceschool.net/03%20machine%20learning/14.03%20%ED%8A%B9%EC%A7%95%20%EC%84%A0%ED%83%9D.html#id2)



### 2.높은 상관관계의 컬럼제거
   - 상관관계가 1.0인 컬럼에 대해 같은 그룹내에서 하나만 남기고 나머지를 제거하는 작업을 하려고 했는데 columns의 순서쌍과 상관관계를 list에 담으려니 상관관계를 구하는 시간도 오래걸리고 그룹화하는 작업도 매우 번거로웠습니다.
   - 그래서 한번에 모든 컬럼에 대한 상관관계를 구하고 기준을 낮추어 0.9이상의 컬럼을 제거하는 처리를 구현하였고 무려 679개의 컬럼이 삭제 되었습니다.



### 3.VIF계수가 30이하가 될때까지 반복하여 컬럼 제거
   - 컬럼이 제거되기 전인 841개의 컬럼에 대해 VIF계수를 한번 구할때 약 40분 정도가 소요되었는데 많은 컬럼이 삭제되어 약 8분정도가 소요됩니다. 
     그래도 아직 상관계수가 $15e+7$ 라서 가장 높은 VIF계수가 30이 될 때까지 반복해서 VIF계수를 구하는 처리를 구현하였고 지금 실행 중입니다. 결과는 수 시간뒤에 나올 것 같습니다.



### 4.PCA 주성분분석
   - 위의 VIF계수를 구하는 처리가 될 때까지 PCA주성분 분석에 대해 공부해봐야겠습니다.

<br>

### 5.PPT작성

- 7/29 목요일 멘토링 시간에 발표한 내용을 포함하여 8/2 월요일에 발표할 PPT를 작성하였습니다.

