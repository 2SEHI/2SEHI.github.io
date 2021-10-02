---
title:  "[python] 자주사용되는 Built-In Functions과 활용예시 - ✨추가중"
excerpt: "Commonly Used Python Built In Functions"

toc: true
toc_sticky: true

date: 2021-09-17
last_modified_at: 2021-09-22

use_math: true
comments: true

categories:
  - Python
---



# 1.구두점 제거 방법

유효한 팰린드롬 를 공부하다가 문자열에서 구두점을 제거하는 여러가지 방법에 대해 정리 해보았습니다.



```python
sentence = "A man, a plan, a canal: Panama"
```



## 1) 정규식 표현 이용

```python
re.sub(r"[^\w]", "", sentence)
```

AmanaplanacanalPanama



## 2) maketrans(a, b) 이용

`string.punctuation`를 출력해보면 `!"#$%&'()*+,-./:;<=>?@[\]^_\``{|}~`의 구두점이 들어있는데 문자열에서 문자를 하나씩 추출하며 구두점이 아닌 문자만 out 변수에 저장합니다.

```python
import string
out = ''.join([i for i in sentence if i not in string.punctuation])
print(out)
```

A man a plan a canal Panama





## 3) replace(t, "") 이용

이 방법 또한 `string.punctuation`를 이용하는데 구두점문자열을 하나씩 순회하며 문자열에 포함된 구두점은 공백으로 replace해주는 방법입니다.

```python
temp = string.punctuation
for t in temp:
    sentence = sentence.replace(t, "")
print(sentence)
```

A man a plan a canal Panama



## 4) isalnum() 이용

`isalnum()` 은 문자열이 알파벳([a-zA-Z])과 숫자([0-9])로만 구성되었는지 확인하는 파이썬 문자열 메소드입니다. 문자가 영문자 또는 숫자인 경우 True를 반환해줍니다. 이를 이용하여 영문자 또는 숫자인 문자열만 배열에 저장합니다.

```python
out = ''.join([s for s in sentence if s.isalnum()])
print(out)
```

AmanaplanacanalPanama





# 2.is 와 == 의 차이

is는 id 주소값을 비교하는 함수이며 ==는 값을 비교하는 연산자입니다.

```python
a = [1, 2, 3]
# 얕은 복사
b = a.copy()
# 깊은 복사
c = a
print('a와 b는 id가 다릅니다 : ', a is b)
print('a와 b는 값이 같습니다 : ', a == b)
print('a와 c는 id가 같습니다 : ', a is c)
```



# 3.locals()

- 클래스 메소드 내부의 모든 로컬 변수를 출력해 변수명을 일일이 찾아낼 필요 없이 잘못 선언된 부분이 없는지 확인하는 용도로 활용할 수 있습니다.



# 4.collections.deque()

- stack과 queue를 변형한 형태로 양쪽끝에서 추가(append)와 삭제(pop)가 가능합니다.

https://docs.python.org/3/library/collections.html#collections.deque

- `append`(*x*) : deque의 오른쪽에 *x*를 추가합니다.

- `appendleft`(*x*) : 데크의 왼쪽에 *x*를 추가합니다.

- `count`(*x*) : *x* 와 같은 데크 요소의 수를 셉니다.

- `insert`(*i*, *x*) : *x*를 데크의 *i* 위치에 삽입합니다.

- `pop`() : 데크의 오른쪽에서 요소를 제거하고 반환합니다. 요소가 없으면, [`IndexError`](https://docs.python.org/ko/3/library/exceptions.html#IndexError)를 발생시킵니다.

- `popleft`() : 데크의 왼쪽에서 요소를 제거하고 반환합니다. 요소가 없으면, [`IndexError`](https://docs.python.org/ko/3/library/exceptions.html#IndexError)를 발생시킵니다.

- `maxlen` : 데크의 최대 크기.설정하지 않으면 길이 변경됩니다.



# 5. strip(chars)

- 지정한 chars가 문자열 앞뒤에 존재할 경우 제거하여 반환해 줍니다.

  `strip(chars)`



## 1) 공백 제거

````python
sentence = '  a  '
print(sentence.strip(' '))

## 실행결과
'''
a
'''
````



## 2) 문자 제거

````python
sentence = '  a..a..'
print(sentence.strip('.'))

## 실행결과
'''
  a..a
'''
````



## 3) lstrip() 선행문자만 제거

````python
sentence = '..a..a..'
print(sentence.lstrip('.'))

## 실행결과
'''
a..a..
'''
````



## 4) rstrip() 후행문자만 제거

````python
sentence = '..a..a..'
print(sentence.rstrip('.'))

## 실행결과
'''
..a..a
'''
````



# 6.zip

`zip()` 함수는 여러 개의 순회 가능한(iterable) 객체를 인자로 받고, 각 객체가 담고 있는 원소를 튜플의 형태로 차례로 접근할 수 있는 반복자(iterator)를 zip object 주소 값으로 반환합니다. 즉, 원소를 튜플의 형태만 만들어 놓고 list나 dict같은 iterable 함수로 감쌀때 실제 값을 만들어 메모리를 소비합니다

```python
zip(["김","이","박"], "kim","Lee","Park")

## 실행결과
'''
<zip at 0x7f432deff6e0>
'''
```



## 1) iterable 객체 묶기



**이때 주의할 점은 iterable 객체의 길이가 다르면 가장 짧은 인자를 기준으로 데이터를 묶고, 나머지는 버려집니다.**

```python
numbers = [1, 2, 3]
letters = ["A", "B", "C"]
for pair in zip(numbers, letters):
     print(pair)

## 출력결과
'''
(1, 'A')
(2, 'B')
(3, 'C')
'''
```



만약에, zip 을 사용하지 않고 위와 같이 출력하려면 다음과 같이 구현해야 합니다.

```python
numbers = [1, 2, 3]
letters = ["A", "B", "C"]
for i in range(3):
     pair = (numbers[i], letters[i])
     print(pair)

## 출력결과
'''
(1, 'A')
(2, 'B')
(3, 'C')
'''
```



## 2) iterable 객체를 list로 만들기

튜플로 묶어주는 것을 list로 담을 수 있습니다.

```python
numbers = [1, 2, 3]
letters = ["A", "B", "C"]
num_letters = list(zip(numbers, letters))
print(num_letters)

## 출력결과
'''
[(1, 'A'), (2, 'B'), (3, 'C')]
'''
```



## 3) iterable 객체를 dictionary로 만들기

list대신에 dictionary 로 담으면 손쉽게 dictionary를 만들 수 있습니다.

```python
numbers = [1, 2, 3]
letters = ["A", "B", "C"]
num_letters = dict(zip(numbers, letters))
print(num_letters)

## 출력결과
'''
{1: 'A', 2: 'B', 3: 'C'}
'''
```



## 4) 별표(*)사용 : 2차원배열을 세로단위로 묶기

2차 배열에 대해 `zip(*2차배열)`와 같이 *을 붙여서 인자값으로 설정하면, 각 행의 열값을 묶어줍니다. 이를 list로 변환해서 사용합니다.

```python
test = [[1,2,3], [2,3,4], [9,8,7]]

list(zip(*test))

## 출력결과
'''
[(1, 2, 9), (2, 3, 8), (3, 4, 7)]
'''
```



[사용예시 보러가기]()



# 7.unpacking

- 💦추가설명이나 예시 필요

위에서 zip은 요소를 튜플의 형태만 만들어 놓고 주소를 보관한다고 했습니다. 그래서 원래의 iterable의 형태로 다시 돌려놓는 것이 가능합니다.

```python
givenName = zip(["김","이","박"], ["kim","Lee","Park"])

zipped_list = list(zip(["김","이","박"], ["kim","Lee","Park"]))
print(zipped_list)

unpack_list = list(zip(*zipped_list))
print(unpack_list)

## 출력결과
'''
[('김', 'kim'), ('이', 'Lee'), ('박', 'Park')]
[('김', '이', '박'), ('kim', 'Lee', 'Park')]
'''
```



