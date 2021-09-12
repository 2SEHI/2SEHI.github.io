---
title:  "[python] lambda표현식"
excerpt: ""

toc: true
toc_sticky: true

date: 2021-09-12
last_modified_at: 2021-09-12

use_math: true
comments: true

categories:
  - Python
---



*“파이썬_코딩도장”을 읽고 정리한 내용입니다.*



# lambda

- lambda표현식은 함수를 간편하게 작성할 수 있어서 다른 함수의 인수로 넣을 때 주로 사용합니다



## 1.lambda 작성

다음과 같이 함수를 lambda 표현식으로 작성할 수 있습니다.

- 함수 표현식

```python
def plus_ten(x):
	return x + 10
```

- lambda 표현식

```python
lambda x : x + 10
```



## 2.lambda 호출

lambda는 이름없는 함수이기 때문에 변수`plus_ten`에 lambda 함수를 할당해주고, 변수의 매개변수에 함수에 대입할 값을 설정해주면 됩니다.

````python
plus_ten = lambda x : x + 10
plus_ten(1)

# 출력결과
```
11
```
````



또는, 변수에 할당하지 않고 lambda 표현식 자체를 호출하여 인수를 넣어서 호출해도 됩니다.

```python
(lambda x : x + 10)(1)
```





## ⛔lambda 안에서의 변수생성 X

lambda 표현식에서는 새 변수를 만들수 없어서 반환값 부분은 변수 없이 식 한줄로 표현할 수 있어야 합니다. 대신 아래와 같이 lambda 표현식 바깥에서 선언된 변수는 lambda내에서 사용 가능합니다. 

```
y = 10
(lambda x : x + y)(1)
```



## 3.map과의 사용

`map(함수, 매개변수 값)` 의 첫번째 인자에 lambda함수를 설정하면, 매개변수 값이 lambda 함수에 할당되어 함수의 반환값이 map객체에 담아지게 됩니다.



````python
plus_ten = lambda x : x + 10
list(map(plus_ten, [1, 2, 3]))

## 출력
```
[11, 12, 13]
```
````





### 1) 조건부 표현식만들기

lambda 표현식에서 lambda 표현식에 if ~ else와 같이 사용하려면, 

`lambda 변수 : 참일 경우 반환값 if 조건식 else 거짓일 경우의 반환값`와 같이 사용하면 되는데 , 이때 주의할 점은 `else`없이`if`만 사용할 수 없고 ,`elif`를 사용하지 못한다는 것입니다.  그래서 `elif`대신에 `if ~else ~ if ~ else`를 연달아 사용하는 방식으로 작성해야 합니다. 그래서 `elif`를 사용해야 하는 경우는 lambda보다는 def 함수에서 `if elif else`를 사용하는 것이 낫습니다.

a 리스트에서 3의 배수를 문자열로 변환하는 방법은 다음과 같습니다.

````python
a = [1, 2, 3, 4, ,5, 6, 7, 8, 9, 10]
list(map(lambda x : str(x) if x % 3 == 0 else x, a))

## 출력 결과
```
[1, 2, '3', 4, 5, '6', 7, 8, '9', 10]
```
````



### 2) map에 여러개의 객체 설정

lambda 에서 필요한 변수가 2개 일 때는 map의 매개변수를 2개로 지정하면 됩니다.

````python
a = [1, 2, 3, 4, 5]
b = [2, 4, 6, 8, 10]
list(map(lambda x, y: x * y, a, b))

## 출력 결과
```
[2, 8, 18, 32, 50]
```
````



## 4.filter와의 사용

filter는 반복 가능한 객체에서 특정 조건에 맞는 요소만 가져오는데 filter에 지정한 함수의 반환값이 True인 경우에만 해당 요소를 가져옵니다.



다음과 같이 5보다 크고 10보다 작은 값인지 True, False로 반환하는 함수가 있다면, filter는 반환값이 True가 되는 요소만 가져오고 False가 되는 요소는 버립니다.

````python
def f(x):
	return x > 5 and x < 10
a = [7, 4, 6, 9, 10, 5]

list(filter(f, a)) # def 함수 표현식
list(filter(lambda x: x > and x < 10, a)) # labmda 함수 표현식


## 출력 결과
```
[7, 6, 9]
```
````



## 5.reduce와의 사용

reduce는 반복 가능한 객체의 각 요소를 지정된 함수로 처리한 뒤 이전 결과와 누적해서 반환하는 함수이며, functools 모듈에서 가져와 사용할 수 있습니다.

````python
from functools import reduce

def f(x, y):
	return x + y

a = [1, 2, 3, 4, 5]
reduce(f, a) # def 함수 표현식
reduce(lambda x, y : x + y, a) # labmda 함수 표현식

## 출력 결과
```
15
```
````



## 6.list표현식

lambda를 사용하지 않고 list표현식으로도 반복문을 처리할 수 있습니다.

### for if 문을 list로 표현

`[참일 경우의 식 for 리스트의 요소 in 리스트 if 조건식1 ]`

````python
a = [1, 2, 3, 4, 5]
[str(i) for i in a  if i % 2 ==0 ]

# 출력 결과
```
['2', '4']
```
````



### for if else문을 list로 표현

`[참일 경우의 식 if 조건식 else 거짓일 경우의 식 for 리스트의 요소 in 리스트 ]`

````python
a = [1, 2, 3, 4, 5]
[str(i) if i % 2 ==0 else i for i in a]

# 출력 결과
```
[1, '2', 3, '4', 5]
```
````



