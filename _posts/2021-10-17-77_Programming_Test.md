---
title:  "[이것이 코딩테스트다(파이썬)] 게임 개발"

date: 2021-10-17
last_modified_at: 2021-10-17

use_math: true
comments: true

categories:
  - Programming Test

tag:
  - 이것이 코딩테스트다(파이썬)

---

# 게임 개발

- 이것이 코딩테스트다(파이썬) 
  - 실전문제 4-4 게임 개발 118p
- level2
- Language : Python



## Python 소스코드

- [4-4.py](https://github.com/2SEHI/Python-Programming-Test/blob/main/python-for-coding-test/4-4.py)

```python
'''
이것이 코딩 테스트다 파이썬
p118
3.게임 개발
'''


# x,y : 위치 좌표
# pd : 현재 방향
# dt : 이동위치 맵
# arr : 육지, 바다 맵
def solution(x, y, pd, dt, arr):
    # 방문한 칸의 개수를 저장할 변수
    result = 1
    # 현재의 위치를 가본 곳이라고 저장
    dt[x][y] = 1
    # Up, Right, Down, Left
    # 북, 서, 남, 동
    direction = [(-1, 0), (0, 1), (1, 0), (0, -1)]
    # TODO 확인용 start
    # 화살표 배열
    arrow = ['↑', '→', '↓', '←']
    # 현재 좌표 방문 처리
    dt[x][y] = arrow[pd]
    # 화살표 출력
    for row in dt:
        print(row)
    print('\n')
    # TODO 확인용 end
    # 회전 횟수
    turn_time = 0

    while True:
        # 현재의 방향을 1감소
        pd -= 1
        # 0보다 작으면 3으로 저장
        if pd < 0:
            pd = 3

        # 이동할 위치 저장
        sx = x + direction[pd][0]
        sy = y + direction[pd][1]
        # TODO 확인용 start
        # 현재 좌표 방문 처리
        dt[x][y] = arrow[pd]
        # 화살표 출력
        for row in dt:
            print(row)
        print('\n')
        # TODO 확인용 end
        # 안가본 곳이면서 육지인 경우
        if arr[sx][sy] == 0 and dt[sx][sy] == 0:
            # 이동 칸의 개수를 증가
            result += 1
            # 이동 위치를 갱신
            x, y = sx, sy
            # 이동위치 맵에 가본 곳이라고 저장
            dt[x][y] = 1
            # 이동했으므로, 이동 횟수를 0으로 초기화
            turn_time = 0

        else:
            # 가본 곳이거나 바다인 경우
            # 회전 횟수를 증가
            turn_time += 1
        # 4면이 모두 가본 곳이거나 바다인 경우
        if turn_time == 4:
            # 방향을 유지한채 뒤로 한칸 이동
            sx = x - direction[pd][0]
            sy = y - direction[pd][1]

            # 뒤가 바다인 경우, 종료
            if arr[sx][sy] == 1:
                break
            else:
                # 뒤가 바다가 아닌 경우,
                # 뒤로 위치이동하고 회전횟수를 초기화
                x, y = sx, sy
                turn_time = 0

    return result


def main():
    # 2차 배열의 크기 입력받기
    n, m = tuple(map(int, input().split()))

    # 캐릭터의 현재 위치(x,y)와 방향
    x, y, pd = map(int, input().split())

    # n*m 크기의 2차 배열을 생성
    dt = [[0] * m for _ in range(n)]

    # 육지와 맵을 저장할 배열
    arr = []
    # 행의 수만큼 육지(0), 바다(1)을 입력받기
    for _ in range(n):
        arr.append(list(map(int, input().split())))
    # 전체 맵 정보를 입력받기
    result = solution(x, y, pd, dt, arr)
    # 이동한 칸의 수 출력
    print(result)

if __name__ == "__main__":
    main()

```



## ⭐풀이 과정

1. 현재의 위치를 기준으로 이동가능한지 판단해야 하므로 현재위치를 1로 저장
2. 왼쪽으로 돌아야하고 북 서 남 동의 값이 0 1 2 3 이므로 배열의 인덱스에 맞게 이동 값을 tuple로 설정

```python
# 북, 서, 남, 동
direction = [(-1, 0), (0, 1), (1, 0), (0, -1)]
```
3. 북, 서, 남, 동 4면을 모두 확인해야 하므로 확인 횟수를 저장하는 변수가 필요

```
turn_time = 0
```

4. 종료 조건을 먼저 구현
   - 모든 4면이 이동을 했거나 바다인 경우, 뒤로 이동을 하는데 뒤가 바다인 경우 종료

```python
while True:
    
    # 4 방향 모두 갈 수 없는 경우
    if turn_time == 4:
        # 방향을 유지한채 뒤로 한칸 이동
        sx = x - direction[pd][0]
        sy = y - direction[pd][1]

        # 뒤가 바다인 경우, 종료
        if arr[sx][sy] == 1:
            break
        else:
        # 뒤가 바다가 아닌 경우, 
        # 뒤로 위치이동하고 회전횟수를 초기화
            x, y = sx, sy
            turn_time = 0
```



5. 현재위치에서 4면을 모두 확인하고 안가본 곳이면서 육지인 경우와 가본 곳이거나 바다인 경우를 나눔

```python
while True:
    # 현재의 방향을 1감소
    pd -= 1
    # 0보다 작으면 3으로 저장
    if pd < 0:
        pd = 3

	# 이동할 위치 저장
    sx = x + direction[pd][0]
    sy = y + direction[pd][1]
    # 안가본 곳이면서 육지인 경우
    if arr[sx][sy] == 0 and dt[sx][sy] == 0:
        # 이동 칸의 개수를 증가
        result += 1
        # 이동 위치를 갱신
        x, y = sx, sy
        # 이동위치 맵에 가본 곳이라고 저장
        dt[x][y] = 1
        # 이동했으므로, 이동 횟수를 0으로 초기화
        turn_time = 0

    else:
        # 가본 곳이거나 바다인 경우
        # 회전 횟수를 증가
        turn_time += 1
```



## note.

이번 문제에서 중요한 것은 **이동했던 위치를 저장하는 배열**과 **육지와 바다를 저장하는 배열** 을 **분리**한다는 점인 것 같습니다.

- **이동했던 위치를 저장하는 배열**과 **육지와 바다를 저장하는 배열** 을 **분리** 하여 안가본 곳이면서 육지인 경우에 이동
- 갈 수 있는 방향이 없는 경우는 뒤쪽 방향이 바다인지만 체크



육지와 바다맵이 아래와 형태인 경우에는 왼쪽 하단 (3, 1)의 좌표로는 이동하지 않아, 육지 칸의 개수는 7개이지만 이동한 칸의 개수는  6이 나옵니다. 

저자의 소스코드로도 실행해보니 같은 결과가 나오는데 모든 위치를 탐색하도록 하는 조건이 아니라서 그런 것 같습니다.

육지이면서 아직 안가본 곳이 존재하면 그 좌표로 이동하는 방식으로 하면 가능할 것 같은데 풀다보니 수업에서 배운 강화학습의 방식도 비슷하다고 느꼈습니다.

```python
array = [[1, 1, 1, 1, 1],
      	[1, 0, 1, 0, 1],
      	[1, 0, 0, 0, 1],
      	[1, 0, 1, 0, 1],
      	[1, 1, 1, 1, 1]]
```

