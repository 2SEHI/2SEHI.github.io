---
title:  "[Programming_Test] LinkedList를 이용한 패린드롬 확인"

date: 2021-09-06
last_modified_at: 2021-09-06

use_math: true
comments: true

categories:
  - Programming Test
---



# LinkedList를 이용한 패린드롬 확인

- 파이썬 알고리즘 인터뷰
- level1

- Language : Python




## [💡문제 보러 가기](https://leetcode.com/problems/palindrome-linked-list/)





## LinkedList 구현

```python
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = None
```



## LinkedList 값 설정

```python
if __name__ == "__main__":
    list1 = ListNode(1)
    list2 = ListNode(2)
    list3 = ListNode(3)
    list4 = ListNode(2)
    list5 = ListNode(1)

    head = list1
    list1.next = list2
    list2.next = list3
    list3.next = list4
    list4.next = list5
```



### 값을 설정한 후의 LinkedList 구조

![57_K-Digital_Training_Project_1](\assets\images\57_K-Digital_Training_Project_1.png)





## 패린드롬여부를 확인하는 함수

```python
def is_palindrome(head: ListNode) -> bool:
    rev = None
    slow = fast = head
    while fast and fast.next:
        fast = fast.next.next
        rev, rev.next, slow = slow, rev, slow.next
    if fast :
        slow = slow.next
    while rev and rev.val == slow.val :
        slow, rev = slow.next, rev.next
```



### rev, slow, fast 초기화

```
    rev = None
    slow = fast = head
```

![57_K-Digital_Training_Project_2](\assets\images\57_K-Digital_Training_Project_2.png)



## fast가 끝에 도달할 때까지 반복

```python
while fast and fast.next:
    fast = fast.next.next
    rev, rev.next, slow = slow, rev, slow.next
```



### 1번째

![57_K-Digital_Training_Project_3](\assets\images\57_K-Digital_Training_Project_3.png)



### 2번째

![57_K-Digital_Training_Project_4](\assets\images\57_K-Digital_Training_Project_4.png)



### 3번째

![57_K-Digital_Training_Project_5](\assets\images\57_K-Digital_Training_Project_5.png)



## fast가 끝에 도달

데이터가 홀수개일 경우(fast가 아직 None이 아닐 때) 정 가운데 데이터는 비교할 필요가 없으므로  slow를 한번더 next해줍니다.

```python
    if fast :
        slow = slow.next
```

![image-20210906014717860](\assets\images\57_K-Digital_Training_Project_6.png)



## rev와 slow 비교

```python
    while rev and rev.val == slow.val :
        slow, rev = slow.next, rev.next
    return not rev
```

### 1번째

![image-20210906014815237](\assets\images\57_K-Digital_Training_Project_7.png)



### 2번째

![image-20210906014931673](\assets\images\57_K-Digital_Training_Project_8.png)



### 3번째

![image-20210906014950355](\assets\images\57_K-Digital_Training_Project_9.png)



## 전체 소스

```python
# LinkedList생성
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = None

## 빠른 러너와 느린 러너를 이용한 패린드롬 확인함수
def is_palindrome(head: ListNode) -> bool:
    # reverse변수
    rev = None
    slow = fast = head
    while fast and fast.next:
        fast = fast.next.next
        rev, rev.next, slow = slow, rev, slow.next
    if fast :
        slow = slow.next
    while rev and rev.val == slow.val :
        slow, rev = slow.next, rev.next
    return not rev
```

