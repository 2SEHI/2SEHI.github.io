---
title:  "[Programming Test] DFS/BFS"

date: 2021-10-18
last_modified_at: 2021-10-19

use_math: true
comments: true

categories:
  - Programming Test
---



# DFS/BFS

## 1.그래프 표현 방식

### 1) 인접 행렬 방식(Adjacency Matrix)

- 2차원 배열로 그래프의 연결 관계를 표현하는 방식

- 모든 관계를 저장하므로 노드 개수가 많을수록 메모리 낭비가 심합니다
- 노드의 연결 확인 속도는 빠릅니다

```python
INF = 999999999
graph = [
    [0, 7, 5],
    [7, 0, INF],
    [5, INF, 0]
]
print(graph)
```



### 2) 인접 리스트(Adjacency List)

- 리스트로 그래프의 연결 관계를 표현하는 방식
- 연결된 정보만을 저장하여 메모리를 효율적으로 사용합니다
- 특정한 두 노드가 연결되어 있는지 확인하려면 연결된 데이터를 하나씩 확인해야 하므로 속도가 느립니다

```python
graph = [[] for _ in range(3)]

graph[0].append((1,7))
graph[0].append((2,5))

graph[1].append((0,7))

graph[2].append((0,5))

print(graph)
```



## 2.DFS(Depth First Search, 깊이 우선 탐색)

![dfs](/assets/images/81_Programming_Test_1.png)

- 특정한 경로로 탐색하다가 특정한 상황에서 최대한 깊숙이 들어가서 노드를 방문한 후, 다시 돌아가 다른 경로로 탐색하는 알고리즘입니다.
- DFS 알고리즘을 구현할 때는 스택 또는 재귀함수를 이용합니다.





### 1) 깊이 우선 탐색의 종류

![tree_problem](/assets/images/81_Programming_Test_2.png)



#### 전위 순회(Preorder)

- 노드방문 -> 왼쪽자식 -> 오른쪽자식
- A -> B -> D -> H -> E -> I -> J -> C -> F -> K -> L -> G



#### 중위 순회(Inorder)

- 왼쪽자식 -> 노드방문 -> 오른쪽자식
- H -> D -> B -> I -> E -> J -> A -> K -> F -> L -> C -> G



#### 후위 순회(Postorder)

- 왼쪽자식 -> 오른쪽자식 -> 노드방문
- H -> D -> I -> J -> E -> B -> K -> L -> F -> G -> C -> A





### 2) 스택을 이용한 동작과정

1. 탐색 시작 노드를 스택에 삽입하고 방문 처리합니다.

2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 넣고 방문 처리합니다. 

   방문하지 않은 인접 노드가 없다면 스택에서 최상단 노드를 꺼냅니다.

3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복합니다.



### 3) 재귀 함수를 이용

- 재귀 함수를 잘 활용하면 복잡한 알고리즘을 간결하게 작성할 수 있습니다.
  - 단, 오히려 다른 사람이 이해하기 어려운 형태의 코드가 될 수도 있으므로 신중하게 사용해야 합니다
- 모든 재귀 함수는 반복문을 이용하여 동일한 기능을 구현할 수 있습니다.
- 재귀 함수가 반복문보다 유리한 경우도 있고 불리한 경우도 있습니다
- 컴퓨터가 함수를 연속적으로 호출하면 컴퓨터 메모리 내부의 스택 프레임이 쌓입니다.
  - 그래서 스택을 사용해야 할 때 구현상 스택 라이브러리 대신에 재귀함수를 이용하는 경우가 많습니다.

![image-20211018164444487](/assets/images/81_Programming_Test_5.png)

`1->2->7->6->8->3->4->5`



```python
def dfs(graph, v, visited):
    # 현재 노드를 방문처리
    visited[v] = True
    # 방문한 노드를 출력
    print(v, end=' ')
    # 현재 노드의 자식 노드를 탐색
    for i in graph[v]:
        # 아직 방문하지 않은 노드라면 방문하기
        if not visited[i]:
            dfs(graph, i, visited)

graph = [
    [],
    [2,3,8],
    [1,7],
    [1,4,5],
    [3,5],
    [3,4],
    [7],
    [2,6,8],
    [1,7]
]
# 각 노드가 방문된 정보를 표현(1차원 리스트)
visited = [False] * 9

# 정의된 DFS 함수 호출
dfs(graph, 1, visited)
```

-  리스트의 1번 인덱스는 1번 노드와 인접한 노드의 번호를 저장하는데 0번 인덱스는 비워두는 것이 좋습니다.
- 1번 노드부터 8번 노드를 가지고 있으므로 0번 노드를 처리하지 않도록 하기 위해 크기가 9인 visited를 선언합니다.





## 3.BFS(Breadth First Search)

![bfs](/assets/images/81_Programming_Test_3.png)

- BFS 는 너비 우선 탐색이라고도 부르며, 그래프에서 가까운 노드부터 우선적으로 탐색하는 알고리즘입니다.
- 큐 자료구조를 이용합니다
- 특정 접근에서의 최단 경로를 구하는데 사용되기도 합니다.



### 1) 큐을 이용한 동작과정

1. 탐색 시작 노드를 큐에 삽입하고 방문 처리합니다.
2. 큐에서 노드를 꺼낸 뒤에 해당 노드의 인접 노드 중에 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리합니다.

3. 2번의 과정을 더 이상 수행  수 없을 때까지 반복합니다.

![image-20211018170043100](/assets/images/81_Programming_Test_4.png)

1->2->3->8->7->4->5->6

```python
from collections import deque

def bfs(graph, start, visited):
    # 현재 노드를 deque에 저장
    queue = deque([start])
    # 현재 노드를 방문 처리
    visited[start] = True
    # 1
    # 2 3 8
    while queue :
        v = queue.popleft()
        print(v, end=' ')
        for i in graph[v]:
            # 방문하지 않은 노드라면 queue에 모두 저장하고 방문처리
            if not visited[i]:
                queue.append(i)
                visited[i] = True
                
graph = [
    [],
    [2,3,8],
    [1,7],
    [1,4,5],
    [3,5],
    [3,4],
    [7],
    [2,6,8],
    [1,7]
]
# 각 노드가 방문된 정보를 표현(1차원 리스트)
visited = [False] * 9

# 정의된 BFS 함수 호출
bfs(graph, 1, visited)
```



## 4.문제 풀이(추가예정)



### 1) 얼음 얼려 먹기

- 이것이 코딩테스트다



#### 풀이방법

- 알고리즘

  - 0 : 구멍이 뚫린 부분이고, 1 : 칸막이

  - 모든 맵의 요소에 대해 확인

  - 요소가 칸막이(1)일 경우는 넘어가고, 구멍(0)일 경우에는 상하좌우에 0으로 연결되어 있는지 확인



- 재귀함수의 파라미터
- return 값
- 종료 조건





#### python 코드 

```python
def dfs(x, y):
    # 맵을 벗어나는 경우, False 반환
    if x <= -1 or x >= n or y <= -1 or y >= m:
        return False
    # 아직 방문하지 않았을 경우
    if graph[x][y] == 0:
        # 방문으로 표시
        graph[x][y] = 1
        # 상하좌우를 확인
        dfs(x-1, y)
        dfs(x+1, y)
        dfs(x, y-1)
        dfs(x, y+1)
        # True 를 반환
        return True
    # 방문한 경우, False 를 반환
    return False


if __name__ == '__main__':
    # 맵의 크기를 입력받기
    n, m = map(int, input().split())
    
    graph = []
    # 맵의 행길이(n)만큼 열(m)의 값을 입력받기
    for i in range(n):
        graph.append(list(map(int, input().split())))

    result = 0
    # 전체 맵에 대해 상하좌우를 확인
    for i in range(n):
        for j in range(m):
            # 구멍이 뚫린 부분은 True 를 반환하므로 
            # 그 개수가 아이스크림의 개수가 됨
            if dfs(i, j):
                result += 1
    print(result)

```



### 2) 미로 탈출

- 이것이 코딩테스트다



### 3) 전위, 중위, 후외 순회 출력

- 문제 보러가기 : [https://www.acmicpc.net/problem/1991](https://www.acmicpc.net/problem/1991)
- 백준



### 4) Z 모양 탐색

- 문제 보러가기 : [https://www.acmicpc.net/problem/1074](https://www.acmicpc.net/problem/1074)

- 백준



### 5) 중량제한

- 문제 보러가기 : [https://www.acmicpc.net/problem/1939](https://www.acmicpc.net/problem/1939)
- 백준



### 6) 타겟 넘버

- 문제 보러가기 : [https://programmers.co.kr/learn/courses/30/lessons/43165](https://programmers.co.kr/learn/courses/30/lessons/43165)
- 프로그래머스

