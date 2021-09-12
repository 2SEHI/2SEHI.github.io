---

title:  "[데이터전처리]2.결측치 처리"
excerpt: "결측치란 무엇인지, 확인방법과 처리방법에 대해 알아봅니다."

toc: true
toc_sticky: true

date: 2021-07-26
last_modified_at: 2021-07-26
use_math: true
comments: true

categories:
  - Machine Learning
---



# 결측치(Missing Data)란?

- 결측치(Missing Data)는 **존재하지 않는 데이터**라는 의미로 숫자 0과는 다릅니다. 
- None, NaN와 같이 표현하는데 머신러닝 알고리즘은 None을 다룰 수없으므로 알고리즘 수행 전에 **결측치를 제거**하거나 **다른값으로 대체**하는 결측치 처리를 해야합니다.
- pandas에서는 결측치에 대한 표현이 없어서 **numpy의 NaN을 이용**합니다. 
- 데이터를 파일로 읽어들일 때 파일내의 #N/A, NA, NULL, NaN등의 문자열은 NaN으로 취급됩니다.
- pandas의 데이터를 읽어오는 함수들에는** na_values라는 옵션**이 있는데 이 옵션에 **값들을 설정하면 그 값들은 None으로 처리**할 수 있습니다.



## 1.결측치 확인방법

- 결측치를 제거, 대체하려면 결측치가 있나 확인을 해야겠죠? 결측치를 확인하는데에는 밑에 세 가지 방법이 있습니다. 나중에 다른 방법을 알게되면 추가하도록 하겠습니다.
    - info()
    - value_counts()
    - isnull이나 notnull



### 1) info()

- DataFrame의 info()를 출력하면 전체 인덱스 개수와 열의 데이터 개수를 확인할 수 있는데 그 2개의 데이터 개수가 다르면 None이 포함된 열입니다.
- RangeIndex: 891 entries 부분에서 전체 행의 개수가 891이라는 것을 알 수 있고, 인덱스 3번의 age, 7번의 embarked,11번의 deck 컬럼 이외에는 891개의 행으로 결측치가 없다는 것을 알 수 있습니다.
- 반면,인덱스 3번의 age에는 177개, 7번의 embarked는 2개, 11번의 deck 컬럼에는 688개의  NaN이있습니다.


```python
import pandas as pd
df = pd.read_csv('./data/titanic.csv')
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 15 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   survived     891 non-null    int64  
     1   pclass       891 non-null    int64  
     2   sex          891 non-null    object 
     3   age          714 non-null    float64
     4   sibsp        891 non-null    int64  
     5   parch        891 non-null    int64  
     6   fare         891 non-null    float64
     7   embarked     889 non-null    object 
     8   class        891 non-null    object 
     9   who          891 non-null    object 
     10  adult_male   891 non-null    bool   
     11  deck         203 non-null    object 
     12  embark_town  889 non-null    object 
     13  alive        891 non-null    object 
     14  alone        891 non-null    bool   
    dtypes: bool(2), float64(2), int64(4), object(7)
    memory usage: 92.4+ KB



### 2) value_counts()

- 특정 열의 value_counts()를 dropna=False옵션과 함께 출력하면 NaN를 포함한 각 데이터의 개수를 출력할 수 있습니다.



```python
df['age'].value_counts(dropna=False)
```




    NaN      177
    24.00     30
    22.00     27
    18.00     26
    28.00     25
            ... 
    0.42       1
    34.50      1
    66.00      1
    14.50      1
    0.92       1
    Name: age, Length: 89, dtype: int64



### 3) isnull()이나 notnull()
- isnull(): NaN이면 True, NaN이 아니면 False를 반환합니다.
- isna() : isnull()과 사용법이 **완전히 같습니다.** 그럼에도 굳이 두가지가 존재하는 이유는 R언어에서는 na와 null이 다른 의미를 가지는데 pandas가 R언어기반으로 만들어져 isna()와 isnull() 두가지 다 쓰여지게 되었다고 합니다. [isnull()과 isna()의 차이점](https://datascience.stackexchange.com/questions/37878/difference-between-isna-and-isnull-in-pandas)
- notnull() : NaN이면 False, NaN가 아니면 True를 반환합니다.
- isnull()과 sum()을 사용하여 특정 컬럼에 NaN이 몇개 존재하는지 알 수 있습니다.


```python
print(df['age'].isnull())
print()
print('age의 NaN개수 : ', df['age'].isnull().sum())
```

    0      False
    1      False
    2      False
    3      False
    4      False
           ...  
    886    False
    887    False
    888     True
    889    False
    890    False
    Name: age, Length: 891, dtype: bool
    
    age의 NaN개수 :  177


- 어느 컬럼에 결측치가 있는지 확인했으면 이제 **결측치를 제거**하거나 **다른 값으로 대체**를 해야 하는데 어떤 기준으로 결측치를 제거하고 대체할까요? 이는 결측치의 종류에 따라 나뉩니다.



## 2.결측치의 종류



### 1) MCAR(Missing completely at random)

- 다른 데이터와 전혀 상관없이 누락된 경우 컬럼 삭제한다.



### 2) MAR(Missing at random)

- 다른 정보에 의존해서 누락된 경우, 관측된 값으로부터 결측치를 추정하여 대체한다.
- 결혼 여부를 묻고 자식 여부를 묻는 경우



### 3) NMAR(Not missing at random)

- 완전 의존하는 경우 - 자식여부는 체크했는데 결혼 여부를 체크 안함 - 삭제하면 안됨



## 3.결측치 제거



### 1) 컬럼 제거

- 특정 컬럼에 None값이 너무 많으면 그 많은 결측치를 다른 값으로 대체하기에는 정확한 분석이 어려우므로 그 컬럼은 제거되야 합니다.



### 2) 행 제거

- 특정 컬럼에 None값이 많지 않은 경우 해당 행(데이터)을 제거합니다.



### 3) dropna()로 결측치제거

- 결측치를 제거할 때는 dropna()를 이용합니다
- dropna()는 기본적으로 원본 데이터를 수정하지 않고 결측치가 제거된 DataFrame를 반환합니다.



#### **- dropna()의 매개변수**

- **axis=1**을 설정하면 컬럼을 제거, axis=0은 행을 제거합니다. 기본은 axis=0입니다.
- **subset=['컬럼명']**을 사용하면 해당 컬럼에 결측치가 존재하는 행만 제거할 수 있습니다.
- **inplace=True**를 사용하면 원본 데이터의 결측치를 바로 제거하며, None을 반환합니다.
- **how=all** 은 모든 열의 값이 None인 경우만 제거하고 **how=any** 는 None이 하나라도 있으면 제거합니다.
- **thresh=n**을 axis=1과 같이 설정하면 **n > None이 아닌 데이터의 개수** 인 경우에만 **해당 컬럼을 제거**합니다.
    - deck에는 none이 아닌 데이터가 203개 있습니다.
    - thresh=203 로 설정할 때는 deck컬럼이 제거되지 않지만
    - thresh=204 로 설정하면 deck컬럼이 제거됩니다.


```python
## thresh의 n <= None이 아닌 데이터의 개수
print(df.dropna(axis=1 , thresh=203).info())
print()

## thresh의 n > None이 아닌 데이터의 개수
print(df.dropna(axis=1 , thresh=204).info())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 15 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   survived     891 non-null    int64  
     1   pclass       891 non-null    int64  
     2   sex          891 non-null    object 
     3   age          714 non-null    float64
     4   sibsp        891 non-null    int64  
     5   parch        891 non-null    int64  
     6   fare         891 non-null    float64
     7   embarked     889 non-null    object 
     8   class        891 non-null    object 
     9   who          891 non-null    object 
     10  adult_male   891 non-null    bool   
     11  deck         203 non-null    object 
     12  embark_town  889 non-null    object 
     13  alive        891 non-null    object 
     14  alone        891 non-null    bool   
    dtypes: bool(2), float64(2), int64(4), object(7)
    memory usage: 92.4+ KB
    None
    
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 14 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   survived     891 non-null    int64  
     1   pclass       891 non-null    int64  
     2   sex          891 non-null    object 
     3   age          714 non-null    float64
     4   sibsp        891 non-null    int64  
     5   parch        891 non-null    int64  
     6   fare         891 non-null    float64
     7   embarked     889 non-null    object 
     8   class        891 non-null    object 
     9   who          891 non-null    object 
     10  adult_male   891 non-null    bool   
     11  embark_town  889 non-null    object 
     12  alive        891 non-null    object 
     13  alone        891 non-null    bool   
    dtypes: bool(2), float64(2), int64(4), object(6)
    memory usage: 85.4+ KB
    None



## 4. 결측치 대체

- 특정 컬럼에 NaN값이 많지 않은 경우, NaN값의 앞이나 뒤의 값, 평균, 중앙값, 최빈값으로 대체하거나 분류나 회귀와 같은 머신러닝으로 값을 예측해 NaN값을 대체합니다.(실제로는 분류나 회귀와 같은 머신러닝을 수행해서 값을 예측해서 대체하는 방법을 많이 쓴다고 합니다)  
- 결측치를 대체할 때는 `replace(A, B)`나 `fillna()`를 사용합니다.



### 1) replace(A, B)

1. [replace(A, B)](#--replacea-b)
2. [replace({A:B, C:D})](#--replaceab-cd)
3. [replace([A:B],[C:D])](#--replaceabcd)
4. [regex=True옵션](#--regextrue옵션)



#### - replace(A, B)

- A를 B로 치환
- 데이터 값을 변경해주는 함수로 첫번째 매개변수 A의 값을 두번째 매개변수 B의 값으로 치환합니다.


```python
import numpy as np
## replace(A, B) : A를 B로 치환
df_rep1 = df['age'].replace(np.NaN, 101)
df_rep1.value_counts(ascending=False)
```




    101.00    177
    24.00      30
    22.00      27
    18.00      26
    30.00      25
             ... 
    53.00       1
    55.50       1
    0.92        1
    24.50       1
    70.50       1
    Name: age, Length: 89, dtype: int64



#### - replace({A:B, C:D})

- A를 B로 치환, C를 D로 치환
- dict형태로 설정하면 key가 찾고자 하는 데이터가 되고 value가 수정할 데이터가 됩니다.


```python
#  replace({A:B, C:D}) : A를 B로 치환, C를 D로 치환
df_rep2 = df['age'].replace({np.NaN: 102,24:103})
df_rep2.value_counts(ascending=False)
```




    102.00    177
    103.00     30
    22.00      27
    18.00      26
    19.00      25
             ... 
    0.92        1
    55.50       1
    0.67        1
    24.50       1
    12.00       1
    Name: age, Length: 89, dtype: int64



#### - replace([A:B],[C:D])

- A를 C로 치환, B를 D로 치환
- list의 형태를 첫번째 매개변수와 두번째 매개변수에 대입하는 것도 가능합니다.


```python
# replace([A:B],[C:D]) : A를 C로 치환, B를 D로 치환
df_rep3 = df['age'].replace([np.NaN,12],[100,102])
df_rep3.value_counts(ascending=False)
```




    100.00    177
    24.00      30
    22.00      27
    18.00      26
    19.00      25
             ... 
    80.00       1
    23.50       1
    70.50       1
    0.42        1
    74.00       1
    Name: age, Length: 89, dtype: int64



#### - regex=True옵션

- 추가로 regex=True 를 설정하고 찾고자 하는 값을 설정하는 부분에 정규 표현식을 설정함으로 문자열 치환을 할 수 있습니다. 
- 이부분은 이상치처리 부분에서 활용해보겠습니다.



### 2) fillna()

- NaN값을 NaN값의 앞이나 뒤의 값, 평균, 중앙값, 최빈값으로 설정할 때 사용할 수 있는 함수입니다.



#### - fillna의 매개변수

- method :bfill을 설정하면 NaN값의 뒤의 값을 NaN에 설정하고 ffill을 설정하면 NaN값의 앞의 값을 설정합니다. 
- value=값 : NaN 대신에 넣고 싶은 싶은 평균값, 중앙값, 최빈값들을 설정합니다.
- inplace=True를 사용하면 원본 데이터의 결측치를 바로 제거하며, None을 반환합니다.



#### **- NaN의 뒤의 값으로 대체**


```python
df_fna1 = df['embarked'].copy()

# embarked컬럼의 NaN값 확인
print(df_fna1.iloc[60:63])

print()

# NaN값의 뒤의 값으로 대체
df_fna1.fillna(method='bfill', inplace=True)

# NaN대체후 결과 확인
print(df_fna1.iloc[60:63])
```

    60      C
    61    NaN
    62      S
    Name: embarked, dtype: object
    
    60    C
    61    S
    62    S
    Name: embarked, dtype: object



#### - 최빈값(가장 많이 존재하는 값)으로 대체


```python
df_fna2 = df['embarked'].copy()

print(df_fna2.iloc[60:63])
print()

# embarked컬럼에서 최빈값 찾기
most_embarked = df_fna2.value_counts().idxmax()
print('embarked의 최빈값 : ', most_embarked)

# NaN을 최빈값으로 대체
df_fna2.fillna(most_embarked, inplace=True)

print()
# NaN대체후 결과 확인
print(df_fna2.iloc[60:63])
```

    60      C
    61    NaN
    62      S
    Name: embarked, dtype: object
    
    embarked의 최빈값 :  S
    
    60    C
    61    S
    62    S
    Name: embarked, dtype: object

