---
title:  "[python] sort() 와 sorted()"
excerpt: ""

toc: true
toc_sticky: true

date: 2021-09-15
last_modified_at: 2021-09-17

use_math: true
comments: true

categories:
  - Python
---



# sort() 와 sorted()

python 내장함수에는 리스트를 정렬하기 위한 함수로 sort()와 sorted()가 있습니다. 그 둘의 차이점과 정렬의 기준이 되는 key에 대해 알아보았습니다.



참고 사이트 :[https://docs.python.org/ko/3/howto/sorting.html](https://docs.python.org/ko/3/howto/sorting.html)



## 1.sort()

- `리스트.sort()`의 형태로 사용됩니다.
- 리스트 자체를 정렬하며 반환값은 None입니다.
- 리스트에서만 사용가능하며, 그 이외의 iterable 객체인 dict나 set, tuple 에서 사용할 수 없습니다.

```python
arr = ['abc', 'ced', 'aav', 'dwd']

print(arr.sort())
print(arr)
```

None

 ['aav', 'abc', 'ced', 'dwd']



## 2.sorted()

- `sorted(iterable)`의 형태로 사용됩니다.
- iterable 객체를 복사하여 정렬하며, 반환값은 복사된 iterable 객체입니다. 매개변수로 지정한 원본 iterable 객체는 변경되지 않습니다.
- 리스트뿐만 아니라 그 외의 iterable 객체에서 모두 사용 가능합니다.

```python
arr = ['abc', 'ced', 'aav', 'dwd']

print(sorted(arr))
print(arr)
```

['aav', 'abc', 'ced', 'dwd']

['abc', 'ced', 'aav', 'dwd']



### 1) 매개변수 reverse

sort()와 sorted()는 기본적으로 오름차순 정렬을 합니다. 내림차순으로 정렬하고자 할 때는 매개변수 reserse를 True로 설정해주면 됩니다.

```python
arr = ['abc', 'ced', 'aav', 'dwd']

print(sorted(arr, reverse=True))
```

`['dwd', 'ced', 'abc', 'aav']`





### 2) 매개변수 key

sort()와 sorted() 모두 공통적으로 매개변수에 key를 지정할 수 있는데 key의 역할은 다음 2가지가 있습니다

- 정렬을 하기 전에 각  리스트 요소에 대해 적용할 callable함수 지정
- 정렬의 기준이 될 인덱스 지정



#### 정렬을 하기 전에 각  리스트 요소에 대해 적용할 callable함수 지정

정렬을 하기 전, 문자열에 대해 대/소문자로 구분없이 정렬하고자 하는 경우 문자열을 소문자로 변환하여 정렬합니다.

````python
sorted("This is a test string from Andrew".split(), key=str.lower)
````

`['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']`





만약, 소문자로 변환하지 않는다면, 대소문자의 Ascii 코드가 다르므로에 의해 대문자가 소문자보다 먼저 정렬됩니다.

````python
print(sorted("This is a test string from Andrew".split()))
````

`['Andrew', 'This', 'a', 'from', 'is', 'string', 'test']`





#### 정렬의 기준이 될 인덱스 지정

lambda 함수를 이용하여 column 1을 기준으로 정렬할 수 있습니다.

```python
arr = [['java', '20'],
       ['python', '9'],
       ['c++', '10'],
       ['c#', '10'],
       ['c', '10']]

sorted(arr, key=lambda x : x[1])
```

`[['c++', '10'], ['c#', '10'], ['c', '10'], ['java', '20'], ['python', '9']]`





그런데 이때 주의할 점은 숫자를 기준으로 정렬을 하려고 했는데 숫자가 문자열로 표현된 데이터는 앞글자만을 기준으로 정렬이 되기 때문에 int형으로 변환하여 정렬을 해주어야 숫자를 기준으로 정렬이 됩니다.

```python
arr = [['java', '20'],
       ['python', '9'],
       ['c++', '10'],
       ['c#', '10'],
       ['c', '10']]

sorted(arr, key=lambda x : int(x[1]))
```

`[['python', '9'], ['c++', '10'], ['c#', '10'], ['c', '10'], ['java', '20']]`





만약에 정렬하려는 값이 같은 경우, 두번째 정렬 기준을 설정할 수 있습니다. column 1가 같은 경우는 column 0을 기준으로 정렬하도록 하였습니다.

```python
arr = [['java', '20'],
       ['python', '9'],
       ['c++', '10'],
       ['c#', '10'],
       ['c', '10']]

sorted(arr, key=lambda x : (int(x[1]), x[0]))
```

`[['python', '9'], ['c', '10'], ['c#', '10'], ['c++', '10'], ['java', '20']]`



operator 모듈은  파이썬 built in 함수로 연산에 관한 메소드를 제공합니다. operater의 itemgetter와 attrgetter를 이용하여 요소를 가져올 수도 있습니다.

```python
import operator

arr = {'abc':'1', 'ced':'2', 'aav':'10', 'dwd':'3'}
result = sorted(arr, key=operator.itemgetter(2))
print(result)
```

`['abc', 'ced', 'dwd', 'aav']`



