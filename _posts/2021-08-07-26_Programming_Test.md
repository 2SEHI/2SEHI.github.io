---

title:  "[Programming_Test] 부족한 금액 계산하기"
excerpt: "프로그래머스 level1, Weekly 1주차"

date: 2021-08-07
last_modified_at: 2021-08-07

use_math: true
comments: true

categories:
  - Programming Test
---

# 부족한 금액 계산하기

- Programmers

- Level1

- Language : Python



## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/82612)

<br>

## Python 소스코드

```python
def solution(price, money, count):
    # 횟수에 따른 가격을 list에 저장
    price_list = [price * i for i in range(1,count+1) ]
    # 가진 돈에서 사용한 돈을 뺀다
    remain = money - sum(price_list)
    # 남은 돈이 0보다 작을 경우, 필요한 액수를 반환
    # 남은 돈이 0보다 클 경우,  0을 반환
    answer = abs(remain * (remain < 0))
    return answer
```



### 다른 사람의 풀이

```python
def solution(price, money, count):
    return max(0,price*(count+1)*count//2-money)
```

####  내가 몰랐던 부분

1. 1씩 증가하는 등차수열의 합 공식이용(너무 오랜만...)

   - 이 방법을 사용하면 배열을 만들 필요가 없습니다

   $(price \times 1) + (price \times 2) + ... + (price \times (count -1 )) + (price \times count) $

   = $price * (1 + 2 + ... + (count-1) + count)$

   = $price \times  \frac{ (count + 1) \times  count}{2}$

2. max()두개 이상의 인자를 지정하면 두개의 인자중에서 큰 값으로 반환합니다.



이번문제는 쉬웠지만 수학공식과 내장함수사용의 중요성에 대해 깨달았습니다.
