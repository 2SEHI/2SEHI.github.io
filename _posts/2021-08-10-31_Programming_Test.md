---
title:  "[Programming_Test] 상호 평가"
excerpt: "프로그래머스 위클리 챌린지 2주차"

date: 2021-08-10
last_modified_at: 2021-08-10

use_math: true
comments: true

categories:
  - Programming Test

---

<br>

# 상호 평가

- Programmers

- Level1

- Language : Python

-  위클리 챌린지 2주차



## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/83201)

<br>

## Python 소스코드

```python
def solution(scores):
    # 배열 전치행렬
    trans = list(zip(*scores))
    # 자기 자신의 평가점수 저장
    myself = [scores[i][i] for i in range(len(scores))]
	
    # 평균점수 리스트
    mean_list = []
    # 자기 자신의 평가점수와, 전치행렬을 순환하며 비교하기
    for m, t in zip(myself, trans):
        # 개인이 받은 점수의 합산 저장
        s = sum(t)
        # 받은 점수의 개수
        cnt = len(t)
        # 자기 자신의 점수와 받은 점수의 최고점 비교하고 유일한 최고점인지 비교
        if (m == max(t)) & (t.count(max(t)) == 1):
            # 최고점을 합산에서 제외
            s = s - max(t)
            # 받은 점수의 개수에서 1개 제거
            cnt -= 1
         # 자기 자신의 점수와 받은 점수의 최저점 비교하고 유일한 최저점인지 비교    
        if (m == min(t)) & (t.count(min(t)) == 1):
            # 최고점을 합산에서 제외
            s = s - min(t)
            # 받은 점수의 개수에서 1개 제거
            cnt -= 1
        # 평균점수리스트에 평균계산하여 저장
        mean_list.append(s / cnt)

    li = []
    # 평균 점수에 등급매기기
    for score in mean_list:
        if score >= 90:
            li.append('A')
        elif 80 <= score < 90:
            li.append('B')
        elif 70 <= score < 80:
            li.append('C')
        elif 50 <= score < 70:
            li.append('D')
        else :
            li.append('F')
	# 등급 반환
    return ''.join(li)
```





### 다른 사람의 풀이

```python
from collections import Counter

def solution(scores):
    def rank(score):
        if score >= 90:
            return 'A'
        elif score >= 80:
            return 'B'
        elif score >= 70:
            return 'C'
        elif score >= 50:
            return 'D'
        else:
            return 'F'
    answer = ''
    # transpose
    for i in range(len(scores)):
        for j in range(1, i+1):
            scores[i][i-j], scores[i-j][i] = scores[i-j][i], scores[i][i-j]
    avg = []
    for i, s in enumerate(scores):
        if (max(s) == s[i] or min(s) == s[i]) and Counter(s)[s[i]] == 1:
            avg.append((sum(s) - s[i])/(len(s)-1))
        else:
            avg.append(sum(s)/len(s))

    rank = ''.join(list(map(rank, avg)))
    return rank
```



#### 내가 몰랐던 부분

- map함수``` map(변환 함수, 순회 가능한 데이터)```와 같이 설정해주면 두번째 인자로 넘어온 데이터가 담고 있는 모든 데이터에 변환 함수를 적용하여 다른 형태의 데이터를 반환합니다. pandas나 numpy뿐만 아니라 list에도 적용가능하다는 사실을 몰랐고 아직 map의 함수 적용방법이 익숙하지 않습니다.
- 또한 중복되는 조건문에 대해  ```if (max(s) == s[i] or min(s) == s[i]) and Counter(s)[s[i]] == 1:```와 같이 한줄에 쓸 수 있다는 것도 아주 유용하다고 생각합니다.