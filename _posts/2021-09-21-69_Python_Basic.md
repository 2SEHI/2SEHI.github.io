---
title:  "[python] 재귀함수"
excerpt: "recursive call"

toc: true
toc_sticky: true

date: 2021-09-21
last_modified_at: 2021-09-21

use_math: true
comments: true

categories:
  - Python

---



_"파이썬\_코딩도장"을 읽고 정리한 내용입니다._



# 재귀함수

함수안에서 함수 자기 자신을 호출하는 방식을 재귀 호출(recursive call)이라고 합니다.

알고리즘에 따라서 반복문으로 구현한 코드보다 재귀호출로 구현한 코드가 더 직관적이고 이해하기 쉬운 경우가 많습니다.



## 재귀함수 사용시 주의사항

다음과 같이 hello()메소드 안에서 hello()메소드를 또 부르는 형태로 사용되는데 파이썬에서의 재귀함수는 최대 호출 횟수가 약 1000번으로 제한되어 있어서  1000번이상 호출될 시 RecursionError가 발생합니다. 따라서 반드시 종료 조건을 만들어 주어야 합니다.

```python
def hello(count):
    count += 1
    print('Hello, world!', count)
    hello(count)
hello(0)
```



실행결과

```
Hello, world! 1
....
Hello, world! 963
---------------------------------------------------------------------------
RecursionError                            Traceback (most recent call last)
<ipython-input-20-415cc6d1a447> in <module>()
      4     print('Hello, world!', count)
      5     hello(count)
----> 6 hello(count)

1 frames
... last 1 frames repeated, from the frame below ...

<ipython-input-20-415cc6d1a447> in hello(count)
      3     count += 1
      4     print('Hello, world!', count)
----> 5     hello(count)
      6 hello(count)

RecursionError: maximum recursion depth exceeded while calling a Python object
```



## 종료조건 만들기

```python
def hello(count):
    if count > 4:
        return
    count += 1
    print('Hello, world!', count)
    hello(count)
hello(0)
```

실행 결과

```
Hello, world! 1
Hello, world! 2
Hello, world! 3
Hello, world! 4
Hello, world! 5
```



## 문제1 - 팩토리얼 구하기

입력받은 n에 대하여 팩토리억 결과를 구하는 함수를 만들어야 합니다.

$n \times (n-1) \times (n-2) \times \cdots \times 2 \times 1 $



```python
def factorial(n):
    if n == 1:
        return 1
    return n * factorial(n-1)

factorial(5)
```

실행결과

```
120
```



## 문제2 - 팰린드롬확인

입력받은 문자열 s에 대하여 팰린드롬인지 확인하는 함수를 만들어야 합니다.

```python
def is_palindrome(word):
    if len(word) < 2:
        return True
    if word[0] != word[-1]:
        return False
    return is_palindrome(word[1:-1])

print(is_palindrome('hello'))
print(is_palindrome('level'))
```



## 문제3 - 피보나치 수 구하기

입력받은 1개의 정수에 해당하는 피보나치 수를 출력하는 함수를 만듭니다

```python
def fib(n):
    if n == 0:
        return 0
    elif n == 1 or n==2:
        return 1
    else:
        return fib(n-1) + fib(n-2)

n = int(input("10~30사이의 정수를 입력하세요 : "))
print(fib(n))
```

실행결과

```
10~30사이의 정수를 입력하세요 : 20
6765
```

