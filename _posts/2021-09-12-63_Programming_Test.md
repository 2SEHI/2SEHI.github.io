---
title:  "[Programming_Test] k개 정렬 리스트 병합"

date: 2021-09-12
last_modified_at: 2021-09-12

use_math: true
comments: true

categories:
  - Programming Test
---

# k개 정렬 리스트 병합

- LeetCode



- Level1



- Language : Python


## [💡문제 보러 가기](https://leetcode.com/problems/merge-k-sorted-lists/)


이번 문제는 연결 리스트에 저장된 정렬 힙을 merge하는 것으로 답에 대한 이해과정을 정리하여 공부했습니다.



### 입력값 생성

_입력값은 실제 문제와 아주 조금 다릅니다._

```
[
    [3, 4, 5],
    [1, 3, 4],
    [2, 6]
]
```



```python
class LinkedList:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

if __name__ == "__main__" :
    a1 = LinkedList(3)
    a2 = LinkedList(4)
    a3 = LinkedList(5)    
    a1.next = a2
    a2.next = a3

    b1 = LinkedList(1)
    b2 = LinkedList(3)
    b3 = LinkedList(4)    
    b1.next = b2
    b2.next = b3

    c1 = LinkedList(2)
    c2 = LinkedList(6)  
    c1.next = c2
    
    # lists에 연결 리스트 담기
    lists = []
    lists.append(a1)
    lists.append(b1)
    lists.append(c1)
```



위에 소스를 실행하고나면 lists는 다음과 같이 구성될 것입니다.

<img src="/assets/images/63_Programming_Test_1.jpg" alt="image-20210912202547865" style="zoom:67%;" />

lists에는 `a1, b1, c1`이 담겨있는데 `a1.next` 는 `a2`, `a2.next`는 `a3`으로 연결되어 있습니다.







### heap에 각 연결 리스트의 가장 작은 요소를 저장



```python
# root와 result를 초기화설정
root = result = ListNone(None)
# heap
heap = []

# k개의 연결 리스트의 첫번째 요소를 heap에 저장
for i in range(len(lists)):
    if lists[i]:
        # 각 LinkedList의 첫번째 인자값, 인덱스번호, 
        heapq.heappush(heap, (lists[i].val, i, lists[i]))
```

![image-20210912202547865](/assets/images/63_Programming_Test_2.jpg)


1. lists에 담긴 각 연결 리스트는 이미 오름차순으로 정렬이 되어 있기 때문에 각 연결 리스트에서 가장 작은 요소를 heap에 저장합니다.
2. heappush를 하면 heap에 튜플 요소가 정렬되어 저장이 되는데 for문이 끝나고 나면 heap에 다음과 같은 상태가 됩니다.

````python
print(heap)

## 출력 결과
```
[(1, 1, <__main__.LinkedList at 0x7f9b9d0ab450>),  # b1
 (3, 0, <__main__.LinkedList at 0x7f9b9d0ab490>),  # a1
 (2, 2, <__main__.LinkedList at 0x7f9b9d0ab190>)] # c1
```
````



![image-20210912193540034](/assets/images/63_Programming_Test_3.jpg)



heap 리스트에 1, 2, 3이 아닌 1, 3, 2의 순서로 저장이 되어 있는데 이것은 오류가 아닙니다. heap은 부모노드가 두 자식노드들보다 크지 않은 것을 보장하는데, 자식노드들간의 크기를 보장하지는 않기 때문입니다. 





### heap에서 가장 작은 요소값을 추출하고 다음 노드를 heap에 저장



```python
# heap이 존재하는 동안 순회
while heap:
    # heap에서 가장 작은 요소값 저장
    node = heapq.heappop(heap)
    # heap에서 가장 작은 요소의 index번호를 저장
    idx = node[1]
    # heap에서 가장 작은 요소의 연결 리스트객체를 result.next에 연결
    result.next = node[2]
	
    # result가 result.next를 가리키도록 함
    result = result.next
    # result.next가 존재한다면 heap배열에 result.next를 저장
    if result.next :
        heapq.heappush(heap, (result.next.val, idx, result.next))
```

#### 1번째

heap배열에서 가장작은 요소인 `(1, 1, b1)`를 추출하여 `result.next`에 연결해주고 `b1.next`인 `b2`를 heap에 저장합니다

![image-20210912203345206](/assets/images/63_Programming_Test_4.jpg)



루프를 한번 돌고나면 `heap`에는 `[(2, 2, c1), (3, 0, a1), (3, 1, b2)]`가 들어있을 것이고, `result`는 `b1`을 가리킬 것입니다. 

또한 주목할 것은 `root`는` LinkedList(0) -> b1`의 순서로 연결되어 있습니다.



#### 2번째

heap배열에서 가장작은 요소인 `(2, 2, c1)`를 추출하여 `result.next`에 연결해주고 `c1.next`인 `c2`를 heap에 저장합니다.

![image-20210912204633068](/assets/images/63_Programming_Test_5.jpg)



이번 루프를 돌고나면 `heap`에 `[(3, 0, a1), (6, 2, c1), (3, 1, b2)]`가 들어있을 것이고, `result`는 `c2`을 가리킬 것입니다. 

 `root`는 `LinkedList(0) -> b1 -> c2`의 순서로 연결됩니다.



위의 처리를 정리해보면 roop를 요소의 개수만큼 돌며, 다음과 같은 처리를 행합니다.

1. heap로부터 가장 작은 항목을 pop

2. result에 그 다음으로 작은 요소를 연결

3. heap에 그 다음으로 작은 요소를 저장

4. root의 연결 리스트 끝에 계속해서 가장 작은 요소를 연결





요소의 개수만큼 루프를 돌고 나면 root에는 요소가 작은 순서대로 연결될 것이므로 root.next를 반환합니다.

![image-20210912205646257](/assets/images/63_Programming_Test_6.jpg)





### 전체 소스

[23_mergeKSortedLists.py](https://github.com/2SEHI/Python-Programming-Test/tree/main/python-algorithm-interview/23_merge_k_sorted_lists.py)
