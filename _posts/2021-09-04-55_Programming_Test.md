---
title:  "[Programming_Test] 모음 사전"
excerpt: "프로그래머스 위클리 챌린지 5주차"

date: 2021-09-04
last_modified_at: 2021-09-04

use_math: true
comments: true

categories:
  - Programming Test
---



# 모음 사전

- Programmers

- Language : Python

- 위클리 챌린지 2주차



## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/84512?language=python3)



## Python 소스코드

```python
def solution(word):
    # 주어진 alphabet
    alphabet = ['A', 'E', 'I', 'O', 'U']
    # 단어 사전
    word_dict = []
    # 중복 순열 import
    from itertools import product
	# alphabet의 개수만큼 for문 순회
    for i in range(1, len(alphabet)+1):
        # 1개부터 5개까지의 alphabet의 중복 순열을 만들고 문자열리스트로 반환함
        result = list(map(lambda x: ''.join(x), list(product(alphabet, repeat=i))))
		# 문자열리스트를 순회 하며 단어사전에 저장
        for j in result :
            word_dict.append(j)
  
	# 단어 사전 정렬
    word_dict.sort()
    # 단어 사전으로부터 단어의 순번을 반환하기
    return word_dict.index(word)+1
```





### 다른 사람의 풀이

```python
from bisect import bisect
from itertools import product

CHARACTERS = "AEIOU"
MAX_LENGTH = 5

dictionary = sorted(
    "".join(p) for i in range(1, MAX_LENGTH + 1) for p in product(CHARACTERS, repeat=i)
)


def solution(word):
    return bisect(dictionary, word)
```



### ⭐내가 몰랐던 부분

- bisect : 이분탐색

  - 이분탐색은 최상단 노드부터 탐색하는데, 현재 노드 값과 비교해서 작으면 왼쪽노드를 탐색하고 크면 오른쪽 노드를 탐색하는 알고리즘입니다.
  - list.index는 모든 리스트를 탐색하여 index를 반환하지만 list는 현재 정렬되어 있으므로 이미 정렬된 리스트를 bisect 함수로 찾으면 list.index 보다 빠르게 찾을 수 있습니다. 

  `bisect_left(literable, value) : 왼쪽 인덱스를 구하기`
  
  `bisect_right(literable, value) : 오른쪽 인덱스를 구하기`
  
  

  출처:[https://programming119.tistory.com/196](https://programming119.tistory.com/196) [개발자 아저씨들 힘을모아]