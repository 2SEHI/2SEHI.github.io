---
title:  "[Programming Test] 3진법 뒤집기"

date: 2021-10-08
last_modified_at: 2021-10-08

use_math: true
comments: true

categories:
  - Programming Test

tag:
  - Programmers
---



# 3진법 뒤집기

- Programmers
- level1

- Language : Python




## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/68935)



## Python 소스코드

```python
def solution(n):
    # 3진법으로 변환된 숫자를 담을 리스트
    array = list()
    # 10진법을 3진법으로 변환
    while n > 0:
        array.append(n % 3)
        n = n // 3
    result = 0
    i = 0
    # 3진법을 10진법으로 변환
    while array:
        result = result + array.pop() * (3 ** i)
        i+=1
    return result
```



## ⭐다른 사람의 풀이

```python
def solution(n):
    tmp = ''
    while n:
        tmp += str(n % 3)
        n = n // 3

    answer = int(tmp, 3)
    return answer
```

- 이번 문제는 쉽지만 다른 사람의 풀이를 보던 주에 int 함수를 사용한 것이 신박해서 이 포스팅을 작성하게 되었습니다.
- `int(n진법, n)` : n진법을 10진법으로 변환할 때 사용할 수 있습니다.
  - [int 함수에 대한 설명 보러 가기]()