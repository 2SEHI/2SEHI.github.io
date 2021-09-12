---

title:  "[Programming_Test] 코딩 테스트 개요"
excerpt: "코딩 테스트 출제 경향 분석 및 파이썬 문법"

toc: true
toc_sticky: true

date: 2021-07-24
last_modified_at: 2021-08-06

use_math: true
comments: true

categories:
  - Programming Test
---





# 코딩 테스트 출제 경향 분석 및 파이썬 문법

- 이 포스팅은 나동빈님의 [(이코테 2021 강의 몰아보기) 1. 코딩 테스트 출제 경향 분석 및 파이썬 문법 부수기 영상](https://www.youtube.com/watch?v=m-9pAwq1o3w) 을 보고 작성하였습니다. 몰랐던 것이나 개념이 애매하게 잡혀있던 내용들에 대해서만 정리하였으므로 영상내용이 100% 전부 적혀있지는 않습니다. 



## 1. 국내 사이트
### 1) 백준 온라인 저지

- https://www.acmicpc.net/



### 2) 코드업

- https://codeup.kr/
- 입문자가 하기 좋음



### 3) 프로그래머스

- https://programmers.co.kr/learn/challenges?tab=all_challenges
- IT 대기업 출제문제



### 4) SW Expert Academy

- https://swexpertacademy.com/main/main.do



## 2. 개발 환경

### 1) 온라인
- 리플릿 
  - https://replit.com/learn

- 파이썬 튜터 
  - http://pythontutor.com/



### 2) 오프라인
- pyCharm 
  - https://www.jetbrains.com/ko-kr/pycharm/download/

- Dev C++
  - https://sourceforge.net/projects/orwelldevcpp/



## 3. 소스코드 관리
- 자주 사용하는 알고리즘 코드를 github에 라이브러리화 하는 것이 좋습니다.



## 4. 출제 경향

- 응시시간 2~5시간으로 그리디(쉬운 난이도), 구현, DFS/BFS를 활용한 탐색의 출체 빈도가 높습니다.



## 5. 복잡도
- 시간 복잡도와 공간 복잡도가 낮을수록 좋은 알고리즘입니다.
- 소스가 복잡해보인다는 개념이 아닌 성능 즉, 알고리즘 수행 시간과 메모리 사용량에 대한 분석입니다.



## 6. 빅오 표기법(Big-O Notation)
- 연산횟수에 대해 가장 빠르게 증가하는 항 만을 고려하는 표기법으로 가장 큰 차수만으로도 성능을 알 수 있습니다.



## 7. 알고리즘 설계 Tip
- 보통 알고리즘 수행시간 제한은 1~5초가량입니다.
- 알고리즘 수행시간 제한을 확인하고 데이터의 개수 N의 범위에 따른 시간복잡도를 계산하여 알고리즘을 설계해야 합니다.
- 시간제한이 1초인 문제를 만날 경우, 일반적인 기준
     N의 범위가 500인 경우 : 시간 복잡도가 $O(N^{3})$인 알고리즘을 설계
     N의 범위가 2000인 경우 : 시간 복잡도가 $O(N^2)$인 알고리즘을 설계
     N의 범위가 10000인 경우 : 시간 복잡도가 $O(NlogN)$인 알고리즘을 설계
     N의 범위가 10,000,000인 경우 : 시간 복잡도가 $O(N)$인 알고리즘을 설계



## 8. 알고리즘 문제 해결 과정
1. 지문을 꼼꼼히 읽고 컴퓨터적으로 사고하기
2. 요구사항(복잡도) 분석
3. 문제 해결을 위한 핵심 아이디어 찾기 <-- 핵심 아이디어를 개치한다면, 간결하게 소스코드를 작성할 수 있습니다.
4. 소스코드 설계 및 코딩



## 9. 수행시간 측정 소스코드 - python

```python
import time
start_time = time.time()

end_time = time.time()
print("time:",end_time - start_time)
```



## 10. 리스트 컴프리헨션

#### m * n크기의 2차원 리스트 초기화

```python
array = [[0]*m for _ in range(n)]
```




## 11. 언더바(_)

- 파이썬에서는 반복을 수행하되 반복을 위한 변수의 값을 무시하고자 할 때 언더바(_)를 사용합니다.

```python
for _ in range(5):
    print("Hello World")
```

**실행 결과:**

```
Hello World
Hello World
Hello World
Hello World
Hello World
```



## 12. list

### 1) list관련 메소드

| 함수명    | 사용법                              | 설명                                                         | 시간 복잡도 |
| --------- | ----------------------------------- | ------------------------------------------------------------ | ----------- |
| append()  | 변수명.append()                     | 리스트에 원소를 하나 삽입할 때 사용한다.                     | $O(1)$      |
| sort()    | 변수명.sort()                       | 기본 정렬 기능으로 오름차순으로 정렬한다.                    | $O(NlogN)$  |
|           | 변수명.sort(reverse=True)           | 내림차순으로 정렬한다.                                       |             |
| reverse() | 변수명.reverse()                    | 리스트의 원소순서를 모두 뒤집어 놓는다.                      | $O(N)$      |
| insert()  | insert(삽입 위치 인덱스, 삽입할 값) | 특정한 인덱스 위치에 원소를 삽입할 때 사용한다.              | $O(N)$      |
| count()   | 변수명.count(특정 값)               | 리스트에서 특정한 값을 가지는 데이터의 개수를 셀 때 사용한다. | $O(N)$      |
| remove()  | 변수명.remove(특정 값)              | 특정한 값을 갖는 원소를 제거하는데, 값을 가진 원소가 여러 개면 하나만 제거한다.  모두 제거하기 위해선  별도의 방법을 사용한다. | $O(N)$      |



### 2) list에서 특정값을 가지는 원소를 모두 제거하기

```python
# a에서 remove_set의 원소를 제거
a = [1, 2, 3, 4, 5, 5, 5]
remove_set = {3, 5}

result = [i for i in a if i not in remove_set]
print('result : ', result)
```

**실행 결과 :** 

```
result : [1, 2, 4]
```



## 13. 튜플을 사용하면 좋은 경우
- **서로 다른 성질의 데이터**를 묶어서 관리해야할 때
    - 최단 경로 알고리즘에서는 (비용, 노드번호)의 형태로 튜플 자료형을 자주 사용합니다.
- 데이터의 나열을 해싱(Hashing)의 키 값으로 사용해야할 때
    - 튜플은 변경이 불가능하므로 리스트와 다르게 키 값으로 사용될 수 있습니다.
- 리스트보다 메모리를 효율적으로 사용해야할 때



## 14. 입력 방법
### 1) 공백을 기준으로 구분된 데이터를 입력받을 때

```python
# 입력받을 데이터의 개수 입력
n = int(input())
# 입력받은 각 데이터를 공백을 기준으로 구분하고 int형으로 변환함
data = list(map(int, input().split()))
data.sort(reverse=True)
print(data)
```

**실행 결과 :** 

```
2
10 20 30
[30, 20, 10]
```



### 2) 공백을 기준으로 구분된 데이터의 개수가 많지 않을 때

```python
a, b, c = map(int, input().split())
print(a, b, c)
```

**실행 결과 :** 

```
3개의 숫자를 입력하세요 :  10 20 30
10 20 30
```





### 3) 빠르게 입력 받기

- 입력이 오래 걸릴 경우 시간제한 판정을 받기 때문에 사용자로부터 입력을 최대한 빠르게 받아야 하는 경우 sys.stdin.readline()메소드를 이용합니다.
    - 단, 입력 후 엔터(Enter)가 줄 바꿈 기호로 입력되느로 rstrip()메소드를 함께 사용합니다.
    
```python
import sys
data = sys.stdin.readline()
print(data)
```



## 15. 출력시 줄바꿈 안하기

- end=" "를 설정합니다.



```python
print(7, end=" ")
print(8, end=" ")
```
**실행 결과 :**

```
7 8
```



## 16. f-string

```python
answer = 7
print(f"정답은 {answer}입니다")
```



## 17. in과 not in

- 다수의 데이터를 담는 자료형을 위해 in 과 not in을 사용합니다.
- 리스트, 튜플, 문자열, 딕셔너리에서 사용가능합니다.
- for문이나 if문에서 사용 가능합니다.



### 1) for문에서 사용


```python
# for문에서 사용할 경우
for x in (10, 20,30):
    print(x)
```

**실행 결과 :**

``` 
10
20
30
```



### 2) if문에서 사용

```python
arr = [10, 20, 30]
x = 20
if x in arr:
    print("x in arr")
```

**실행 결과 :**

```
x in arr
```



## 18. pass 

- 아무것도 처리하고 싶지 않은 경우 pass키워드를 이용합니다.



## 19. 조건문 내에서의 부등식

- 파이썬에서는 ```x > 0 and x < 20``` 또는 ```0 < x < 20```  의 부등식 표현이 가능합니다.

```python
x = int(input("숫자를 입력하세요:"))
if 0 < x < 20 :
    print("0 < x < 20")
```

**실행 결과 :**

```
0 < x < 20
```



## 20. 반복문

- while보다는 for문을 사용할 때 소스가 더 간결해집니다.

1. while문의 경우 조건식이 참일 경우에만 while안의 코드블럭을 수행합니다.


```python
i = 1
result = 0

while i <= 9:
    result += i
    i += 1

print(result)    
```

**실행 결과 : **

```
45
```





## 21. global 키워드

- global키워드로 변수를 지정하면 해당 함수에서는 지역 변수를 만들지 않고, 함수 바깥에 선언된 변수를 바로 참조하게 됩니다.

```python
a = 0

def func():
    # 함수에서 a를 사용하기 위해서는 global선언을 해주어야 함
    global a
    a += 1 
    
for i in range(5) :
    func()

print(a)
```

**실행 결과 : **

```
5
```



- 다만 전역 변수와 같은 이름의 변수가 함수내에 선언되면 함수내에 선언된 변수를 참조합니다.

```python
array = [1,2,3,4,5]

def arr():
    array = [10,20]
    # arr()함수내의 array를 호출
    print(array)
    
arr()
# arr()함수바깥의 전역변수 array를 호출
print(array)
```

**실행 결과 : **

```
[10, 20]
[1, 2, 3, 4, 5]
```



## 22. 여러 개의 반환 값

- 파이썬에서의 함수는 여러 개의 값을 반환할 수 있습니다.

```python
def operator(a, b):
    add_var = a + b
    divide_var = a / b
    return add_var, divide_var

print(operator(7,3))
```

**실행 결과 : **

```(10, 2.3333333333333335)```



## 23. 람다 표현식

- 특정 기능의 함수를 한 줄에 작성할 수 있습니다.



### 1) my_key함수 사용할 경우

```python
array = [('홍길동', 50), ('이순신',32),('아무개', 74)]
def my_key(x):
    return x[1]
print(sorted(array, key=my_key))
```

**실행 결과 : **

```
[('이순신', 32), ('홍길동', 50), ('아무개', 74)]
```



### 2) my_key함수를 사용하지 않고 labda를 사용할 경우

```python
array = [('홍길동', 50), ('이순신',32),('아무개', 74)]
print(sorted(array, key=lambda x : x[1]))
```

**실행 결과 : **

```
[('이순신', 32), ('홍길동', 50), ('아무개', 74)]
```



### 3) map함수사용

- 여러개의 리스트에 동일한 식을 적용하고자 할 때 map함수와 lambda를 이용하여 간결하게 구현할 수 있습니다.
- map함수는 각각의 원소에 함수를 적용하고자 할 때 사용합니다.

```python
list1 = [1,2,3,4,5]
list2 = [6,7,8,9,10]
result = map(lambda a,b : a + b, list1, list2)
print(list(result))
```

**실행 결과 : **

```
[7, 9, 11, 13, 15]
```



## 24. 실전에서 유용한 표준 라이브러리


### 1) 내장 함수

- 기본 입력출력 함수부터 정렬 함수까지 기본적인 함수들을 제공합니다.
- sum(), min(), max(), eval(), sorted()
#### - eval()

- 사용자에게 수학 표현식을 문자열로 받고, 계산 결과값을 출력해주는 간단한 함수입니다

```python
result = eval("(3+5)*7")
print(result)
```

**실행 결과 : **

```
56
```



#### - sorted()

- 정렬의 기준을 key에 지정하고, reverse=True는 오름차순 정렬입니다.

```python
array = [('홍길동', 50), ('이순신',32),('아무개', 74)]

result = sorted(array, key=lambda x:x[1], reverse=True)
print(result)
```

**실행 결과 : **

```
[('아무개', 74), ('홍길동', 50), ('이순신', 32)]
```



### 2) itertools

- 파이썬에서 반복되는 형태의 데이터를 처리하기 위한 유용한 기능들을 제공합니다.
- 특히 순열과 조합 라이브러리는 완전탐색문제 유형에서 소스코드를 간결하게 해줘서 코딩테스트에서 자주 사용됩니다.

#### - 순열

- 서로 다른 n개에서 서로 다른 r개를 선택하여 일렬로 나열하는 것
- {'A', 'B', 'C'}에서 세 개를 선택하여 나열하는 경우 :'ABC', 'ACB', 'BAC', 'BCA', 'CAB', 'CBA'

```python
from itertools import permutations
data = ['A', 'B', 'C']
result = list(permutations(data, 2))
print(result)
```
**실행 결과 :** 

```
[('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]
```



#### - 조합

- 서로 다른 n개에서 순서에 상관 없이 서로 다른 r개를 선택하는 것

- {'A', 'B', 'C'}에서 순서를 고려하지 않고 두 개를 뽑는 경우 'AB', 'AC', 'BC'

```python
from itertools import combinations	
data = ['A', 'B', 'C', 'D']

result = list(combinations(data, 3))
print(result)
```

**실행 결과 :**

```
[('A', 'B', 'C'), ('A', 'B', 'D'), ('A', 'C', 'D'), ('B', 'C', 'D')]
```



#### - 중복 순열

- 자기 자신을 포함한 순열

```python
from itertools import product
data = ['A', 'B', 'C']
result = list(product(data, repeat=2))
print(result)
```

실행 결과 : 

```
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'B'), ('B', 'C'), ('C', 'A'), ('C', 'B'), ('C', 'C')]
```



#### - 중복 조합

- 자기 자신을 포함한 조합

```python
from itertools import combinations_with_replacement
data = ['A', 'B', 'C']
result = list(combinations_with_replacement(data, 2))
print(result)
```

실행 결과 : 

```
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]
```



### 3) heapq
- 힙(heap) 자료구조를 제공합니다.
- 일반적으로 우선순위 큐 기능을 구현하기 위해 사용됩니다.
- 다익스트라와 같은 최단 경로 알고리즘에서 많이 사용됩니다.

### 4) bisect

- 이진 탐색(Binary Search)기능을 제공합니다.



### 5) collections
- 덱(deque), 카운터(Counter)듣의 유용한 자료구조를 포함합니다.

#### - Counter

- 등장 횟수를 세는 기능을 제공합니다
- iterable객체가 주어졌을 때 내부의 원소가 몇 번씩 등장했는지를 알려줍니다.

```python
from collections import Counter
counter = Counter(['red', 'blue', 'red', 'green', 'blue', 'blue'])

print(counter['blue']) # blue의 출현 횟수
print(counter['green']) # green의 출현 횟수
print(dict(counter)) # dict형태로 반환
```

**실행 결과 :** 

```
3
1
{'red': 2, 'blue': 3, 'green': 1}
```



### 6) math

- 필수적인 수학적 기능을 제공합니다.
- 팩토리얼, 제곱근, 최대공약수(GCD), 삼각함수 관련 함수부터 파이(pi)와 같은 상수를 포함합니다.



#### - gcd()

- 최대 공약수를 구하는 함수입니다.

```python
import math

# gcd함수를 이용하여 최대 공배수를 구하는 함수
def lcm(a, b):
    return a * b // math.gcd(a, b)

a = 21
b = 14

# 최대 공약수 구하기
print(math.gcd(21, 14))
# 최대 공배수 구하기
print(lcm(21, 14))
```

**실행 결과 :**

```
7
42
```



## 25. 참 거짓 판정에 따른 flag반환

```px = 1 * (pixels > avg) ```

- pixels > avg가 참이면 1반환

  pixels > avg가 거짓이면 0을 반환

-  px엔 pixels > avg의 판정 결과에 따라 1 또는 0이 설정됩니다.



## 26. yield와 Generate
