---
title:  "[Programming Test] 크레인 인형뽑기 게임 - 2019카카오 인턴쉽"

date: 2021-10-02
last_modified_at: 2021-10-02

use_math: true
comments: true

categories:
  - Programming Test

tag:
  - Programmers
---



# 크레인 인형뽑기 게임 - 2019카카오 인턴쉽

- Programmers
- level1

- Language : Python




## [💡문제 보러 가기](https://programmers.co.kr/learn/courses/30/lessons/64061)



## Python 소스코드

 **programmers에 돌릴 때는 print출력문이 많으면 에러가 나서 print문을 빼고 돌려야 합니다.**

```python
# programmers에 돌릴 때는 print문을 빼고 돌려야 됨
def solution(board, moves):
    stack = []
    answer = 0
    # moves 배열 크기만큼 순회
    while moves :
        # moves배열의 맨 앞 요소를 인덱스로 저장
        idx  = moves.pop(0)-1
        # 인형을 저장할 변수
        target = 0
        # 크레인 배열 만큼 순회
        for b in board :
            # 빈칸이 아니면 인형변수에 저장하고 크레인에서 빈칸으로 저장
            if b[idx] != 0:
                target, b[idx] = b[idx], 0
                break
            # 모두 빈칸인 경우에 아무것도 하지 않고 다음으로 넘어가기
            if target == 0:
            	continue
        # 인형바구니 stack이 비어있지 않다면 target과 stack의 마지막 요소를 비교
        if len(stack) > 0 :
            if stack[-1] == target:
                print("target(", target, ") == stack의 마지막 요소(", stack[-1],") 이므로 제거 => ", end=" ")
                stack.pop()
                answer += 2
            else :
                print("target(", target, ") != stack의 마지막 요소(", stack[-1],") 이므로 저장 => ", end=" ")
                stack.append(target)
        else :
            # 인형바구니 stack이 비어있다면 target을 stack에 저장
            print("stack(empty) target (", target,") 저장 => ", end=" ")
            stack.append(target)
        print("stack의 상태 : ", stack, end="\n")
    return answer
```



1. filter 를 이용하여 빈칸(0)이 아닌 요소를 찾고 싶었는데 그 요소를 찾더라도 인덱스값이 없으면 빈칸(0)으로 만들 수가 없어서 for if 를 이용해서 찾았습니다.

2. 문제에서 **만약 인형이 없는 곳에서 크레인을 작동시키는 경우에는 아무런 일도 일어나지 않습니다**  라는 제약조건이 있었는데 이에 대한 처리를 추가하고 나니  test case 1과 2이 통과되었습니다.



### ⭐다른 사람의 풀이

```python
def solution(board, moves):
    stacklist = []
    answer = 0

    for i in moves:
        for j in range(len(board)):
            if board[j][i-1] != 0:
                stacklist.append(board[j][i-1])
                board[j][i-1] = 0

                if len(stacklist) > 1:
                    if stacklist[-1] == stacklist[-2]:
                        stacklist.pop(-1)
                        stacklist.pop(-1)
                        answer += 2     
                break

    return answer
```

target이라는 변수에 따로 인형을 저장하여 stack과 비교하였는데 인형을 발견한 즉시 stack에 넣고 있습니다.

target을 stack에 바로 넣지 않고 stack의 마지막요소와 비교한 뒤에 stack에 추가하려면 같은 경우, 같지 않은 경우 에 대한 처리를 따로 해주어야 하는데 이 분과 같이 일단 stacklist에 인형을 추가하고 난뒤에 -1번째와 -2번째를 비교하여 같으면 pop을 하는 방식이라 비교적 코드가 간결해보입니다.



### ⭐다른 사람의 풀이

```python
def solution(board, moves):
    cols = list(map(lambda x: list(filter(lambda y: y > 0, x)), zip(*board)))
    a, s = 0, [0]

    for m in moves:
        if len(cols[m - 1]) > 0:
            if (d := cols[m - 1].pop(0)) == (l := s.pop()):
                a += 2
            else:
                s.extend([l, d])

    return a
```

제가 사용하고 싶었던 filter를 이용하여 풀으셨는데 한줄에 map과 filter를 같이 써서 가독성이 좋은 것같지는 않습니다. 

그리고 이 소스에서 zip을 사용한 방식이 이해가 가지 않아서 알아보았습니다.
