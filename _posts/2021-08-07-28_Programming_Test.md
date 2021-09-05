---
title:  "[Programming_Test] 음양 더하기"
excerpt: "프로그래머스 level1"

date: 2021-08-07
last_modified_at: 2021-08-07

use_math: true
comments: true

categories:
  - Programming Test
---

# 음양 더하기

- Programmers

- Level1

- Language : Python



## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/76501#)

<br>

## Python 소스코드

```python
def solution(absolutes, signs):
    return sum([-absolutes[i] if signs[i] == False else absolutes[i] for i in range(len(absolutes)) ])
```



### 다른 사람의 풀이

```
def solution(absolutes, signs):
    return sum(absolutes if sign else -absolutes for absolutes, sign in zip(absolutes, signs))
```

#### 내가 몰랐던 부분

- 같은 길이의 list는 zip을 이용하여 같은 인덱스끼리 잘라 for문으로 원소를 순환할 수 있습니다. zip을 사용하면 i 인덱스로 각 리스트의 인덱스 i를 지정하지 않아도 됩니다.
