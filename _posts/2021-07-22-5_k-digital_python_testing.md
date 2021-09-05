---
title:  "Python 분석라이브러리 활용 문제 풀이"
excerpt: "numpy, pandas, seaborn등을 활용한 문제 풀이"

toc: true
toc_sticky: true
 
date: 2021-07-22
last_modified_at: 2021-07-22

---

## 빅데이터 기반 AI 응용 솔루션 개발자 전문과정
### 교과목명 : Python 분석라이브러리 활용
- 평가일 : 21.7.22


```python
import pandas as pd 
import numpy as np
import seaborn as sns
```

> ## Q1. arange(), reshape() 이용 1차원 2차원 3차원 배열을 아래와 같이 생성하세요.

    [0 1 2 3 4 5 6 7 8 9]
    [[0 1 2 3 4]
    [5 6 7 8 9]]
    [[[0 1 2 3 4]
    [5 6 7 8 9]]]


```python
# 답
print(np.arange(10))
print(np.arange(10).reshape(2,5))
print(np.arange(10).reshape(1,2,5))
```

    [0 1 2 3 4 5 6 7 8 9]
    [[0 1 2 3 4]
     [5 6 7 8 9]]
    [[[0 1 2 3 4]
      [5 6 7 8 9]]]
    

---

> ## Q2. 1 ~ 100 까지 배열에서 5의 배수이면서 2의 배수인 것만을 출력하세요.


```python
# 답
for i in range(1,101):
    if (i % 5 == 0) & (i % 2 == 0):
        print(i)
```

    10
    20
    30
    40
    50
    60
    70
    80
    90
    100
    

---

> ## Q3. 아래 3차원 배열을 생성하여 출력한 후 1차원으로 변환하여 출력하세요.

    [[[ 0 1 2 3 4]
    [ 5 6 7 8 9]]
    [[10 11 12 13 14]
    [15 16 17 18 19]]
    [[20 21 22 23 24]
    [25 26 27 28 29]]]



```python
# 답
arr3d = np.arange(30).reshape(3,2,5)
print(arr3d)
print(arr3d.flatten())
```

    [[[ 0  1  2  3  4]
      [ 5  6  7  8  9]]
    
     [[10 11 12 13 14]
      [15 16 17 18 19]]
    
     [[20 21 22 23 24]
      [25 26 27 28 29]]]
    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
     24 25 26 27 28 29]
    

---
> ## Q4. array2d에서 인덱스를 이용해서 값을 선택하고 리스트로 아래와 같이 출력하세요.

    arr2d = np.arange(1,10).reshape(3,3)
    [3, 6]
    [[1, 2],
    [4, 5]]
    [[1, 2, 3]
    [4, 5, 6]



```python
arr2d = np.arange(1,10).reshape(3,3)
arr2d
```




    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])




```python
# 답
print(arr2d[:2,2])
print(arr2d[:2,:2])
print(arr2d[:2,:])
```

    [3 6]
    [[1 2]
     [4 5]]
    [[1 2 3]
     [4 5 6]]
    

---

> ## Q5. 아래 두행렬을 np.arange, reshape를 이용해서 생성 각각 a1, b1으로 저장하고 행렬 내적을 계산한 결과를 출력하세요

    [[1 2 3]
    [4 5 6]
    [7 8 9]]
    [[10 11]
    [12 13]
    [14 15]]



```python
# 답
a1 = np.arange(1,10).reshape(3,3)
b1 = np.arange(10,16).reshape(3,2)
# 내적 행렬
a1.dot(b1)
```




    array([[ 76,  82],
           [184, 199],
           [292, 316]])



---
> ## Q6. [[1,2,3],[11,12,13]] 리스트와 array([[ 1, 2, 3],[11, 12, 13]]) 배열을 대입하고 컬럼명을 'col1','col2','col3'으로 하는 데이터프레임을 작성하세요



```python
# 답
# 리스트
list1 = [[1,2,3],[11,12,13]]
array2 = np.array(list1)
columns =['col1','col2','col3']
newArr = pd.concat([pd.DataFrame(list1),pd.DataFrame(array2)])
newArr.rename(columns={0:'col1',1:'col2',2:'col3'},inplace=True)
newArr
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
      <th>col1</th>
      <th>col2</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>12</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



---
> ## Q7. 10 ~ 20 사이의 정수 난수로 10행 5열 2차원 행렬을 생성하고 컬럼명이 A B C D E인 데이터프레임을 작성하세요



```python
# 답
rArr = np.random.randint(10, 20, size=(10, 5))
pd.DataFrame(rArr, columns=['A','B','C','D','E'])
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>17</td>
      <td>11</td>
      <td>14</td>
      <td>10</td>
      <td>14</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16</td>
      <td>16</td>
      <td>17</td>
      <td>13</td>
      <td>16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15</td>
      <td>10</td>
      <td>16</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>13</td>
      <td>16</td>
      <td>12</td>
      <td>11</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>15</td>
      <td>17</td>
      <td>17</td>
      <td>11</td>
    </tr>
    <tr>
      <th>5</th>
      <td>18</td>
      <td>10</td>
      <td>12</td>
      <td>17</td>
      <td>14</td>
    </tr>
    <tr>
      <th>6</th>
      <td>14</td>
      <td>13</td>
      <td>11</td>
      <td>14</td>
      <td>15</td>
    </tr>
    <tr>
      <th>7</th>
      <td>14</td>
      <td>15</td>
      <td>11</td>
      <td>17</td>
      <td>18</td>
    </tr>
    <tr>
      <th>8</th>
      <td>18</td>
      <td>10</td>
      <td>10</td>
      <td>12</td>
      <td>19</td>
    </tr>
    <tr>
      <th>9</th>
      <td>18</td>
      <td>19</td>
      <td>19</td>
      <td>16</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>



---
> ## Q8. df = sns.load_dataset('titanic')로 불러와서 다음 작업을 수행한 후 출력하세요.
- 전체 칼럼중 'survived'외에 모든 칼럼을 포함한 df_x를 산출한 후 dataset/df_x.pkl로 저장한다.
- df_x.pkl을 데이터프레임 df_x 이름으로 불러온 후 앞 5개 행을 출력한다.


```python
df = sns.load_dataset('titanic')
df_x = df.drop(labels='survived', axis=1)

df_x.to_pickle("/dataset/df_x.pkl")

df_x2 = pd.read_pickle("/dataset/df_x.pkl")
df_x2.head(5)
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
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>embarked</th>
      <th>class</th>
      <th>who</th>
      <th>adult_male</th>
      <th>deck</th>
      <th>embark_town</th>
      <th>alive</th>
      <th>alone</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>C</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Cherbourg</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>S</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>53.1000</td>
      <td>S</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>



---
> ## Q9. df = sns.load_dataset('titanic')로 불러와서 deck 열에서 NaN 갯수를 계산하세요.


```python
df = sns.load_dataset('titanic')
df['deck'].isnull().sum()
```




    688



---
> ## Q10. Q9의 df에서 각 칼럼별 null 개수와 df 전체의 null 개수를 구하세요.


```python
df.isnull().sum()
```




    survived         0
    pclass           0
    sex              0
    age            177
    sibsp            0
    parch            0
    fare             0
    embarked         2
    class            0
    who              0
    adult_male       0
    deck           688
    embark_town      2
    alive            0
    alone            0
    dtype: int64



> ## 아래 tdf 데이터프레임에서 Q11 ~ Q12 작업을 수행하세요.


```python
import seaborn as sns
df = sns.load_dataset('titanic')
tdf = df[['survived','sex','age','class']]
tdf.head()
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
      <th>survived</th>
      <th>sex</th>
      <th>age</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>male</td>
      <td>22.0</td>
      <td>Third</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>First</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>female</td>
      <td>26.0</td>
      <td>Third</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>female</td>
      <td>35.0</td>
      <td>First</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>male</td>
      <td>35.0</td>
      <td>Third</td>
    </tr>
  </tbody>
</table>
</div>



---
> ## Q11. age를 7개 카테고리로 구분하는 새로운 칼럼 'cat_age'를 생성하여 출력하세요.
단, 카테고리 구분을 수행하는 사용자 함수를 만들고 그 함수를 age 칼럼에 매핑하여 결
과를 tdf1에 저장하고 출력하세요.

    [카테고리]
    age <= 5: cat = 'Baby'
    age <= 12: cat = 'Child'
    age <= 18: cat = 'Teenager'
    age <= 25: cat = 'Student'
    age <= 60: cat = 'Adult'
    age > 60 : cat = 'Elderly'


```python
import math
def ageCategorical(age):
    cat = 'Elderly'
    if math.isnan(age):
        cat = np.nan
    else :         
        age = int(age)
        if age <= 5: 
            cat = 'Baby'
        elif age > 5 and age <= 12: 
            cat = 'Child'
        elif age > 12 and age <= 18:
            cat = 'Teenager'
        elif age > 18 and age <= 25: 
            cat = 'Student'
        elif age > 25 and age <= 60: 
            cat = 'Adult'
        elif age > 60:
            cat = 'Elderly'
    return cat

tdf1= tdf.copy()
tdf1['age_cat'] = tdf1['age'].apply(ageCategorical)
tdf1['age_cat'] = pd.Categorical(tdf1['age_cat'], categories=bins, ordered=False)
tdf1
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
      <th>survived</th>
      <th>sex</th>
      <th>age</th>
      <th>class</th>
      <th>age_cat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>male</td>
      <td>22.0</td>
      <td>Third</td>
      <td>Student</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>First</td>
      <td>Adult</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>female</td>
      <td>26.0</td>
      <td>Third</td>
      <td>Adult</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>female</td>
      <td>35.0</td>
      <td>First</td>
      <td>Adult</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>male</td>
      <td>35.0</td>
      <td>Third</td>
      <td>Adult</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>886</th>
      <td>0</td>
      <td>male</td>
      <td>27.0</td>
      <td>Second</td>
      <td>Adult</td>
    </tr>
    <tr>
      <th>887</th>
      <td>1</td>
      <td>female</td>
      <td>19.0</td>
      <td>First</td>
      <td>Student</td>
    </tr>
    <tr>
      <th>888</th>
      <td>0</td>
      <td>female</td>
      <td>NaN</td>
      <td>Third</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>889</th>
      <td>1</td>
      <td>male</td>
      <td>26.0</td>
      <td>First</td>
      <td>Adult</td>
    </tr>
    <tr>
      <th>890</th>
      <td>0</td>
      <td>male</td>
      <td>32.0</td>
      <td>Third</td>
      <td>Adult</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 5 columns</p>
</div>



---
> ## Q12. tdf1의 sex, class 칼럼을 '_'으로 연결한 'sc'칼럼을 추가한 후 아래와 같이 출력하세요.



```python
# 출력 결과
tdf1['sc'] = tdf1['sex'] + '_' +tdf1['class'].astype(str)
tdf1
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
      <th>survived</th>
      <th>sex</th>
      <th>age</th>
      <th>class</th>
      <th>age_cat</th>
      <th>sc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>male</td>
      <td>22.0</td>
      <td>Third</td>
      <td>Student</td>
      <td>male_Third</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>First</td>
      <td>Adult</td>
      <td>female_First</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>female</td>
      <td>26.0</td>
      <td>Third</td>
      <td>Adult</td>
      <td>female_Third</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>female</td>
      <td>35.0</td>
      <td>First</td>
      <td>Adult</td>
      <td>female_First</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>male</td>
      <td>35.0</td>
      <td>Third</td>
      <td>Adult</td>
      <td>male_Third</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>886</th>
      <td>0</td>
      <td>male</td>
      <td>27.0</td>
      <td>Second</td>
      <td>Adult</td>
      <td>male_Second</td>
    </tr>
    <tr>
      <th>887</th>
      <td>1</td>
      <td>female</td>
      <td>19.0</td>
      <td>First</td>
      <td>Student</td>
      <td>female_First</td>
    </tr>
    <tr>
      <th>888</th>
      <td>0</td>
      <td>female</td>
      <td>NaN</td>
      <td>Third</td>
      <td>NaN</td>
      <td>female_Third</td>
    </tr>
    <tr>
      <th>889</th>
      <td>1</td>
      <td>male</td>
      <td>26.0</td>
      <td>First</td>
      <td>Adult</td>
      <td>male_First</td>
    </tr>
    <tr>
      <th>890</th>
      <td>0</td>
      <td>male</td>
      <td>32.0</td>
      <td>Third</td>
      <td>Adult</td>
      <td>male_Third</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 6 columns</p>
</div>



---
> ## Q14. 아래 df에서 drop_duplicates() 메소드를 사용하여 c2, c3열을 기준으로 중복 행을 제거한 후 df3에 저장하고 출력하세요.


```python
# 중복 데이터를 갖는 데이터프레임 만들기
import pandas as pd
df = pd.DataFrame({'c1':['a', 'a', 'b', 'a', 'b'],
 'c2':[1, 1, 1, 2, 2],
 'c3':[1, 1, 2, 2, 2]})
print(df)
```

      c1  c2  c3
    0  a   1   1
    1  a   1   1
    2  b   1   2
    3  a   2   2
    4  b   2   2
    


```python
df3 = df.drop_duplicates(subset=['c2', 'c3'])
print(df3)
```

      c1  c2  c3
    0  a   1   1
    2  b   1   2
    3  a   2   2
    

---
> ## Q15. 'mpg'를 'kpl' 로 환산하여 새로운 열을 생성하고 처음 3개 행을 소수점 아래 둘째자리에서 반올림하여 출력하세요.


```python
# read_csv() 함수로 df 생성
import pandas as pd
auto_df = pd.read_csv('/content/drive/MyDrive/Colab Notebooks/data/auto-mpg.csv')
# 열 이름을 지정
auto_df.columns = ['mpg','cylinders','displacement','horsepower','weight',
 'acceleration','model year','origin','name'] 
print(auto_df.head(3))
```

        mpg  cylinders  displacement  ... model year  origin                name
    0  15.0          8         350.0  ...         70       1   buick skylark 320
    1  18.0          8         318.0  ...         70       1  plymouth satellite
    2  16.0          8         304.0  ...         70       1       amc rebel sst
    
    [3 rows x 9 columns]
    


```python
auto_df['kpl'] = round((auto_df['mpg'] * 1.609344)/3.785411784, 2)
auto_df['kpl'].head(3)
```




    0    6.38
    1    7.65
    2    6.80
    Name: kpl, dtype: float64



---
> ## Q16. './dataset/stock-data.csv'를 데이터프레임으로 불러와서 datetime64 자료형으로 변환한 후에 년, 월, 일로 분리하고 year를 인덱스로 셋팅하여 출력하세요.


```python
df = pd.read_csv('/content/drive/MyDrive/Colab Notebooks/data/stock-data.csv')
df['Date'] = pd.to_datetime(df['Date'])
year_index = df['Date'].map(lambda x : x.year)
df = df.set_index(year_index)
df
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
      <th>Date</th>
      <th>Close</th>
      <th>Start</th>
      <th>High</th>
      <th>Low</th>
      <th>Volume</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018</th>
      <td>2018-07-02</td>
      <td>10100</td>
      <td>10850</td>
      <td>10900</td>
      <td>10000</td>
      <td>137977</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-29</td>
      <td>10700</td>
      <td>10550</td>
      <td>10900</td>
      <td>9990</td>
      <td>170253</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-28</td>
      <td>10400</td>
      <td>10900</td>
      <td>10950</td>
      <td>10150</td>
      <td>155769</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-27</td>
      <td>10900</td>
      <td>10800</td>
      <td>11050</td>
      <td>10500</td>
      <td>133548</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-26</td>
      <td>10800</td>
      <td>10900</td>
      <td>11000</td>
      <td>10700</td>
      <td>63039</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-25</td>
      <td>11150</td>
      <td>11400</td>
      <td>11450</td>
      <td>11000</td>
      <td>55519</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-22</td>
      <td>11300</td>
      <td>11250</td>
      <td>11450</td>
      <td>10750</td>
      <td>134805</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-21</td>
      <td>11200</td>
      <td>11350</td>
      <td>11750</td>
      <td>11200</td>
      <td>133002</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-20</td>
      <td>11550</td>
      <td>11200</td>
      <td>11600</td>
      <td>10900</td>
      <td>308596</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-19</td>
      <td>11300</td>
      <td>11850</td>
      <td>11950</td>
      <td>11300</td>
      <td>180656</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-18</td>
      <td>12000</td>
      <td>13400</td>
      <td>13400</td>
      <td>12000</td>
      <td>309787</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-15</td>
      <td>13400</td>
      <td>13600</td>
      <td>13600</td>
      <td>12900</td>
      <td>201376</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-14</td>
      <td>13450</td>
      <td>13200</td>
      <td>13700</td>
      <td>13150</td>
      <td>347451</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-12</td>
      <td>13200</td>
      <td>12200</td>
      <td>13300</td>
      <td>12050</td>
      <td>558148</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-11</td>
      <td>11950</td>
      <td>12000</td>
      <td>12250</td>
      <td>11950</td>
      <td>62293</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-08</td>
      <td>11950</td>
      <td>11950</td>
      <td>12200</td>
      <td>11800</td>
      <td>59258</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-07</td>
      <td>11950</td>
      <td>12200</td>
      <td>12300</td>
      <td>11900</td>
      <td>49088</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-05</td>
      <td>12150</td>
      <td>11800</td>
      <td>12250</td>
      <td>11800</td>
      <td>42485</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-04</td>
      <td>11900</td>
      <td>11900</td>
      <td>12200</td>
      <td>11700</td>
      <td>25171</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2018-06-01</td>
      <td>11900</td>
      <td>11800</td>
      <td>12100</td>
      <td>11750</td>
      <td>32062</td>
    </tr>
  </tbody>
</table>
</div>



---
> ## Q17. titanic 데이터셋에서 5개 열을 선택해서 class열을 기준으로 그룹화를 수행한 후 아래와 같이 출력하였다. 다음 사항을 출력하세요.
- 5개 열 : ['age','sex', 'class', 'fare', 'survived']
- 그룹별 평균 출력
- 그룹별 최대값 출력



```python
df = sns.load_dataset('titanic')
columns =  ['age','sex', 'class', 'fare', 'survived']
df1 = pd.DataFrame(df, columns=columns)

print(df1.groupby('class').mean())
print(df1.groupby('class').max())
```

                  age       fare  survived
    class                                 
    First   38.233441  84.154687  0.629630
    Second  29.877630  20.662183  0.472826
    Third   25.140620  13.675550  0.242363
             age   sex      fare  survived
    class                                 
    First   80.0  male  512.3292         1
    Second  70.0  male   73.5000         1
    Third   74.0  male   69.5500         1
    

---
> ## Q18. titanic 데이터셋에서 'Third'그룹만을 선택해서 group3 이름으로 저장하고 통계요약표를 출력하세요.


```python
group3 = df.groupby('class').get_group('Third')
group3
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
      <th>survived</th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>embarked</th>
      <th>class</th>
      <th>who</th>
      <th>adult_male</th>
      <th>deck</th>
      <th>embark_town</th>
      <th>alive</th>
      <th>alone</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>S</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>8.4583</td>
      <td>Q</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Queenstown</td>
      <td>no</td>
      <td>True</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>2.0</td>
      <td>3</td>
      <td>1</td>
      <td>21.0750</td>
      <td>S</td>
      <td>Third</td>
      <td>child</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>882</th>
      <td>0</td>
      <td>3</td>
      <td>female</td>
      <td>22.0</td>
      <td>0</td>
      <td>0</td>
      <td>10.5167</td>
      <td>S</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>True</td>
    </tr>
    <tr>
      <th>884</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>25.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.0500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>True</td>
    </tr>
    <tr>
      <th>885</th>
      <td>0</td>
      <td>3</td>
      <td>female</td>
      <td>39.0</td>
      <td>0</td>
      <td>5</td>
      <td>29.1250</td>
      <td>Q</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Queenstown</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>888</th>
      <td>0</td>
      <td>3</td>
      <td>female</td>
      <td>NaN</td>
      <td>1</td>
      <td>2</td>
      <td>23.4500</td>
      <td>S</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>890</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>32.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.7500</td>
      <td>Q</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Queenstown</td>
      <td>no</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
<p>491 rows × 15 columns</p>
</div>



---
> ## Q19. titanic 데이터셋에서 class 열, sex열 기준으로 그룹화한 후 그룹별 평균과 표준편차를 구하세요.


```python
print(df.groupby(['class','sex']).mean())
print(df.groupby(['class','sex']).std())
```

                   survived  pclass        age  ...        fare  adult_male     alone
    class  sex                                  ...                                  
    First  female  0.968085     1.0  34.611765  ...  106.125798    0.000000  0.361702
           male    0.368852     1.0  41.281386  ...   67.226127    0.975410  0.614754
    Second female  0.921053     2.0  28.722973  ...   21.970121    0.000000  0.421053
           male    0.157407     2.0  30.740707  ...   19.741782    0.916667  0.666667
    Third  female  0.500000     3.0  21.750000  ...   16.118810    0.000000  0.416667
           male    0.135447     3.0  26.507589  ...   12.661633    0.919308  0.760807
    
    [6 rows x 8 columns]
                   survived  pclass        age  ...       fare  adult_male     alone
    class  sex                                  ...                                 
    First  female  0.176716     0.0  13.612052  ...  74.259988    0.000000  0.483070
           male    0.484484     0.0  15.139570  ...  77.548021    0.155511  0.488660
    Second female  0.271448     0.0  12.872702  ...  10.891796    0.000000  0.497009
           male    0.365882     0.0  14.793894  ...  14.922235    0.277674  0.473602
    Third  female  0.501745     0.0  12.729964  ...  11.690314    0.000000  0.494727
           male    0.342694     0.0  12.159514  ...  11.681696    0.272754  0.427207
    
    [6 rows x 8 columns]
    

---
> ## Q20. titanic 데이터셋에서 다음 전처리를 수행하세요.
1. df에서 중복 칼럼 6개를 삭제한 후 df1 이름으로 저장 후 5개 행을 출력하세요.
2. df1에서 null값이 50% 이상인 칼럼을 삭제 후 df2 이름으로 저장하고 출력하세요.
3. df2에서 결측값이 있는 age 칼럼에 대해서 평균값으로 데체 처리를 수행하세요.
4. df2에서 결측값이 있는 embarked 칼럼에 대해서 앞행의 값으로 데체 처리를 수행하세요.
5. df2 문자로 되어있는 칼럼들을 레이블 인코딩 수행하여 숫자로 변환 후 df2.info()를 출력하세요


```python
# 1. df에서 중복 칼럼 6개를 삭제한 후 df1 이름으로 저장 후 5개 행을 출력하세요.
columns = ['survived','pclass','sex','age','sibsp','parch','fare','embarked','deck']
df1 = pd.DataFrame(df,columns=columns)
df1.head(5)
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
      <th>survived</th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>embarked</th>
      <th>deck</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>0</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>53.1000</td>
      <td>0</td>
      <td>C</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 2. df1에서 null값이 50% 이상인 칼럼을 삭제 후 df2 이름으로 저장하고 출력하세요.
df2 = df1.dropna(thresh=(len(df1) / 2), axis=1)
df2
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
      <th>survived</th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>53.1000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>886</th>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>27.0</td>
      <td>0</td>
      <td>0</td>
      <td>13.0000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>887</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>30.0000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>888</th>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
      <td>2</td>
      <td>23.4500</td>
      <td>0</td>
    </tr>
    <tr>
      <th>889</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>30.0000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>890</th>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>32.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.7500</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 8 columns</p>
</div>




```python
# 3. df2에서 결측값이 있는 age 칼럼에 대해서 평균값으로 대체 처리를 수행하세요.
df2['age'] = df2['age'].fillna(value=df2['age'].mean())
print(df2)
df2.info()
```

         survived  pclass  sex        age  sibsp  parch     fare  embarked
    0           0       3    1  22.000000      1      0   7.2500         1
    1           1       1    0  38.000000      1      0  71.2833         0
    2           1       3    0  26.000000      0      0   7.9250         0
    3           1       1    0  35.000000      1      0  53.1000         0
    4           0       3    1  35.000000      0      0   8.0500         1
    ..        ...     ...  ...        ...    ...    ...      ...       ...
    886         0       2    1  27.000000      0      0  13.0000         1
    887         1       1    0  19.000000      0      0  30.0000         0
    888         0       3    0  29.699118      1      2  23.4500         0
    889         1       1    1  26.000000      0      0  30.0000         1
    890         0       3    1  32.000000      0      0   7.7500         1
    
    [891 rows x 8 columns]
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 8 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   survived  891 non-null    int64  
     1   pclass    891 non-null    int64  
     2   sex       891 non-null    int64  
     3   age       891 non-null    float64
     4   sibsp     891 non-null    int64  
     5   parch     891 non-null    int64  
     6   fare      891 non-null    float64
     7   embarked  891 non-null    int64  
    dtypes: float64(2), int64(6)
    memory usage: 55.8 KB
    

    /usr/local/lib/python3.7/dist-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    




```python
# 4. df2에서 결측값이 있는 embarked 칼럼에 대해서 앞행의 값으로 대체 처리를 수행하세요.
df2['embarked'] = df2['embarked'].fillna(method='ffill')
df2.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 8 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   survived  891 non-null    int64  
     1   pclass    891 non-null    int64  
     2   sex       891 non-null    int64  
     3   age       891 non-null    float64
     4   sibsp     891 non-null    int64  
     5   parch     891 non-null    int64  
     6   fare      891 non-null    float64
     7   embarked  891 non-null    int64  
    dtypes: float64(2), int64(6)
    memory usage: 55.8 KB
    

    /usr/local/lib/python3.7/dist-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    


```python
# 5. df2 문자로 되어있는 칼럼들을 레이블 인코딩 수행하여 숫자로 변환 후 df2.info()를 출력하세요
from sklearn.preprocessing import LabelEncoder

labelEcd = LabelEncoder()
df2['sex'] = labelEcd.fit_transform(df2['sex'].values)
df2['embarked'] = labelEcd.fit_transform(df2['sex'].values)
df2.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 8 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   survived  891 non-null    int64  
     1   pclass    891 non-null    int64  
     2   sex       891 non-null    int64  
     3   age       891 non-null    float64
     4   sibsp     891 non-null    int64  
     5   parch     891 non-null    int64  
     6   fare      891 non-null    float64
     7   embarked  891 non-null    int64  
    dtypes: float64(2), int64(6)
    memory usage: 55.8 KB
    

    /usr/local/lib/python3.7/dist-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      """
    /usr/local/lib/python3.7/dist-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    
