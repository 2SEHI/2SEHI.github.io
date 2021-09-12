---

title:  "[Programming_Test] 체육복"
excerpt: "프로그래머스 level1"

date: 2021-08-06
last_modified_at: 2021-08-06

use_math: true
comments: true

categories:
  - Programming Test
---

# 체육복
- Programmers
- Level1
- Language : Python




## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/42862)





## Python 소스코드

```python
def solution(n, lost, reserve):
    # lost에서 reserve와 중복된 사람을 제거하여 저장
    lost_reverse = sorted([i for i in lost if i not in reserve])
    # reserve에서 lost와 중복된 사람을 제거하여 저장
    reverse_lost = sorted([i for i in reserve if i not in lost])
    # 체육복을 빌린 사람을 담는 배열
    borrow_list=[]
  
    for lo in lost_reverse:
        for re in reverse_lost:
		    # 도난 당한 사람과 여분이 있는 사람을 순회하며 빌려줄 수 있는지(앞과 뒤에 있는지) 체크
            if re-1 <= lo <= re+1:
                # borrow_list 배열에 담기
                borrow_list.append(lo)
                # 여유분을 빌려준 사람을 제거하기
                reverse_lost.remove(re)
	# 수업을 들을 수 있는 사람 = 전체사람수 - 도난당한 사람 + 체육복을 빌린 사람 
    answer = n - len(lost_reverse) + len(borrow_list)
    return answer
```

<div style="text-align:center"><img src="\assets\images\25_Physical_training_uniform_1.png" alt="25_Physical_training_uniform_1.png" style="zoom:67%;" /></div>

1. 도난 사람중에 여유분이 있는사람을 제거하여 저장했는데 이부분은 바로 떠올랐지만 도난 사람이 체육복을 빌렸을 때 빌린 사람과 빌려준 사람을 다시 체크 대상이 되게 하지 않기 위해 배열을 추가 및 제거를 해야 했는데 어이없게도 이 처리가 바로 생각 나지 않았습니다.

3. 1번을 해결하고 test case 7번과 13번이 계속 통과하지 못했는데 배열을 정렬하니 문제가 통과되었습니다. 문제 내용중 다음과 같은 말이 있었는데 이를 간과하였습니다

<div style="text-align:center"><p style="color:red;font-weight: bold ">체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.</p></div>



### sort를 할 경우와 sort하지 않을 경우

sort를 할 경우와 sort하지 않았을 때 어떻게 되는지 보면 다음과 같이 sort를 하지 않으면 수업을 들을 수 있는 사람은 4명이 되고, sort하면 5명이 되므로, sort하지 않으면 위에서 언급한  <span style="color:red;font-weight: bold "> 최대한 많은 학생이 체육수업을 들어야 한다</span> 는 조건에 부합하지 않게 됩니다.

<div style="text-align:center"><img src="\assets\images\25_Physical_training_uniform_2.png" alt="25_Physical_training_uniform_2.png" style="zoom:67%;" /></div>



### ⭐다른 사람의 풀이

```python
def solution(n, lost, reserve):
    _reserve = [r for r in reserve if r not in lost]
    _lost = [l for l in lost if l not in reserve]
    for r in _reserve:
        f = r - 1
        b = r + 1
        if f in _lost:
            _lost.remove(f)
        elif b in _lost:
            _lost.remove(b)
    return n - len(_lost)
```

여유분을 가진 사람 리스트를 순회하며 앞과 뒤의 번호를 도난 당한 사람 리스트에서 제거를 하고 앞 번호를 먼저 체크를 함으로써 sort를 하지 않아도 된다는 것도 인상적이었습니다.
