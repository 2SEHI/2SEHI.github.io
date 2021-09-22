---
title:  "[python] 자주사용되는 Built-In Functions - ✨추가중"
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

`string.punctuation`를 출력해보면 `!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~`의 구두점이 들어있는데 문자열에서 문자를 하나씩 추출하며 구두점이 아닌 문자만 out 변수에 저장합니다.

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

is는 id 값을 비교하는 함수이며 ==는 값을 비교하는 연산자입니다.

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
```
a
```
````



## 2) 문자 제거

````python
sentence = '  a..a..'
print(sentence.strip('.'))

## 실행결과
```
  a..a
```
````



## 3) lstrip() 선행문자만 제거

````python
sentence = '..a..a..'
print(sentence.lstrip('.'))

## 실행결과
```
a..a..
```
````



## 4) rstrip() 후행문자만 제거

````python
sentence = '..a..a..'
print(sentence.rstrip('.'))

## 실행결과
```
..a..a
```
````

