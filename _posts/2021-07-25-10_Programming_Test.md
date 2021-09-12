---

title:  "[Programming_Test]로또의 최고 순위와 최저 순위"
excerpt: "프로그래머스 level1"

date: 2021-07-25
last_modified_at: 2021-07-25
use_math: true
comments: true

categories:
  - Programming Test
---



# 로또의 최고 순위와 최저 순위

- Programmers

- Level1

- Language : Python



## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/77484)



*내 나름의  ~~비루한~~ 알고리즘 풀이...*

![10_lotto_lanking](\assets\images\10_lotto_lanking.png)



## Python 소스코드

``` python
from collections import Counter

def solution(lottos, win_nums):
	# 랭킹 초기화
    best_rank = 1
    worst_rank = 6
    
    # 확실한 당첨 번호의 개수
    atari = len([i for i in win_nums if i in lottos])
        
    # zero의 개수 구하기
    zero_cnt = lottos.count(0)
    
    # zero의 개수가 0개일 경우
    if zero_cnt == 0:
        best_rank = rank(atari)
        worst_rank = rank(atari)
    # zero의 개수가 1개 이상 6개 일 경우
    elif 1 <= zero_cnt < 6:
        worst_rank = rank(atari)
        best_rank = rank(zero_cnt + atari)
    return [best_rank, worst_rank]

# ranking 구하는 함수
def rank(count):
    ranking = 6
    if count == 2:
        ranking = 5
    elif count == 3 :
        ranking = 4
    elif count == 4 :
        ranking = 3
    elif count == 5 :
        ranking = 2
    elif count == 6 :
        ranking = 1
    return ranking
```



0의 개수를 구하는것과 lottos와 win_nums의 일치하는 숫자의 개수를 구하는 것은 잘 짰는데 ranking을 구하는데서 불필요한 `if elif`가 많이 들어갔습니다. 다른 사람들의 풀이를 보고나니 더욱더 코딩테스트연습을 열심히 해야겠다는 생각이 들었습니다. 



### 다른사람의 풀이

ranking을 구할 때 if elif 말고도 ranking을 배열로 만들어 zero의 개수와 일치하는 숫자의 개수를 index로 활용할 수도 있습니다.
```python
def solution(lottos, win_nums):

    rank=[6,6,5,4,3,2,1]
    
    cnt_0 = lottos.count(0)
    ans = 0
    for x in win_nums:
        if x in lottos:
            ans += 1
    return rank[cnt_0 + ans],rank[ans]
```



또한, Java로 짜여진 다른사람의 풀이를 보았는데 확실히 파이썬이 간결한 언어라는걸 깨달았습니다. 그리고 Java의 lambda에 대해서도 알고 싶어졌습니다. 앞으로 Python뿐만 아니라 Java나 C언어로도 Coding Test문제를 종종 풀어봐야겠습니다.
