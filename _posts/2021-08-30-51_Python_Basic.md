---
title:  "[python] iterator(이터레이터)"
excerpt: ""

toc: true
toc_sticky: true

date: 2021-08-30
last_modified_at: 2021-08-30

use_math: true
comments: true

categories:
  - Python
---

_"파이썬\_코딩도장"을 읽고 정리한 내용입니다._



# iterator(이터레이터)

- `iterator`는 값을 차례대로 꺼낼 수 있는 object입니다.

`for i in range(100):`은 숫자를 모두 만들어 내는 것이 아니라 `iterator 객체`를 하나 생성하고 for문을 돌때마다 데이터를 만들어내는데 이와 같이 데이터 생성을 뒤로 미루는 방식을 지연 평가(lazy evaluation)라고 합니다.



## \__iter__ : iterator객체 생성

데이터의 타입이 반복가능한 `object` 인지 알기 위해서는 `dir() `메소드로 사용가능한 메소드를 확인하면 되는데 사용가능한 메소드에 `__iter__`가 있다면 반복가능한 `object`입니다. 반복가능한 `object`로는 dict, str, set, list 등이 있습니다.

````python
iterator_set = {3,1,2}
print(dir(iterator_set))

# 출력 결과
```
['__and__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__iand__', '__init__', '__init_subclass__', '__ior__', '__isub__', '__iter__', '__ixor__', '__le__', '__len__', '__lt__', '__ne__', '__new__', '__or__', '__rand__', '__reduce__', '__reduce_ex__', '__repr__', '__ror__', '__rsub__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__xor__', 'add', 'clear', 'copy', 'difference', 'difference_update', 'discard', 'intersection', 'intersection_update', 'isdisjoint', 'issubset', 'issuperset', 'pop', 'remove', 'symmetric_difference', 'symmetric_difference_update', 'union', 'update']
```
````



`__iter__`메소드를 호출하면 아래와같이 `iterator객체`가 출력됩니다.

````python
print(iterator_set.__iter__())

# 출력 결과
```
<set_iterator object at 0x0000024B90431240>
```
````



## \__next__ : 요소 꺼내기

`iterator객체`를 `it`변수에 할당하여 `__next__`메소드로 호출하면 요소를 하나씩 꺼내줍니다. 그리고 더이상 꺼낼 요소가 없으면 StopIteration 예외를 발생시킵니다.

````python
it = iterator_set.__iter__()
print(it.__next__())
print(it.__next__())
print(it.__next__())
print(it.__next__())

# 출력 결과
```
1
2
3
Traceback (most recent call last):

  File "C:\Users\.spyder-py3\temp.py", line 10, in <module>
    print(it.__next__())

StopIteration
```
````



## for문

사실 `for문`을 반복할 때 위와 같이 `__iter__`로 `iterator객체`를 생성하고 `__next__`로 요소를 하나씩 꺼낸 다음 StopIteration 예외가 발생하면 반복을 멈추는 것이었습니다.



`__iter__`와 `__next__`메소드를 모두 가진 객체를 `iterator protocol을 지원한다`라고 말합니다. 이때 주의할 점은 반복 가능한 객체와 iterator는 별개의 객체이므로 구분해야 합니다. **반복 가능한 객체에서 `__iter__`메소드로 `iterator객체`를 얻는 것입니다.**



##### <for문의 동작 과정>

 `__iter__`로 `iterator객체` 얻기

​						:small_red_triangle_down:

 `iterator객체`로부터 `__next__`메소드 호출하여 다음 요소 꺼내기

​						:small_red_triangle_down:

 StopIteration예외 발생하면 종료


## 이터레이터 만들기



`__iter__`메소드와 `__next__`메소드를 가진  Counter클래스를 만들어서  `for문`으로 반복문을 수행해 보았습니다.

```python
class Counter:
    def __init__(self, stop):
        self.current = 0
        self.stop = stop
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current < self.stop:
            r = self.current
            self.current += 1
            return r
        else :
            raise StopIteration # 예외발생

```

````python
for i in Counter(3):
     print(i,end=' ')

# 출력 결과
```
0 1 2
```
````



출력결과가 0 1 2이 나오는데 `__iter__`나 `__next__`메소드를 호출한 적이 없는네 1씩 증가할 수 있는 이유는 바로 앞에서 설명했던 `for문`의 동작과정때문입니다. 만약 Counter클래스내에 `__iter__`나 `__next__`메소드를 지우고 `for문`을 반복해보면 에러가 발생합니다.



### `__iter__`메소드 없이 for문 수행

```python
Traceback (most recent call last):

  File "C:\Users\.spyder-py3\temp.py", line 17, in <module>
    for i in Counter(3):

TypeError: 'Counter' object is not iterable
```



### `__next__`메소드 없이 for문 수행

```python
Traceback (most recent call last):

  File "C:\Users\.spyder-py3\temp.py", line 11, in <module>
    for i in Counter(3):

TypeError: iter() returned non-iterator of type 'Counter'
```



## 이터레이터 언패킹

언패킹이란 결과를 여러 개의 변수에 할당하는 것을 말합니다. `iterator객체`는 결과를 변수 여러 개에 할당할 수 있습니다.

```python
a, b, c = Counter(3)
print(a, b, c)
```



다음과 같이 변수 `_`에 할당하는 경우가 있는데  `_`에 값이 할당되는 특정 순서의 반환값을 사용하지 않고 무시하고자 할 때 사용합니다.

````python
a, _, c = Counter(3)
# _에도 값은 할당되지만 a와 c만 사용
print(a, _, c) 

# 출력 결과
```
0 1 2
```
````



## \__getitem__ : 인덱스 접근

반복가능한 객체는  `[인덱스]`으로 슬라이싱 가능한데 사실 슬라이싱할 때 `__getitem__`메소드에 인덱스를 매개변수로 넘겨주어 인덱스 접근이 가능했던 것입니다.

따라서 `__getitem__ `메소드는 `iterator객체`의 요소에 인덱스로 접근할 수 있습니다.





### 기본적인 문법

```python
class iterator이름:
    def __getitem__(self, 인덱스):
		코드
```



### iterator 클래스 생성

```python
class Counter:
    def __init__(self, stop):
        self.stop = stop

    def __getitem__(self, index):
        if index < self.stop:
            return index
        else : 
            raise IndexError   
```



`iterator클래스`인 Counter에 `__iter__`와 `__next__`가 없어도 `[인덱스]`로 슬라이싱하면 `__getitem__`에 인덱스를 인자값으로 넘겨주어 인덱스를 반환반들수 있습니다.

````python
print(Counter(3)[0], Counter(3)[1], Counter(3)[2])

for i in Counter(3):
    print(i, end=' ')

# 출력 결과
```
0 1 2
0 1 2 
```
````



위에서는 인덱스를 바로 반환했지만 `__ getitem__`메소드를 수정하여 다른 값을 반환하도록 다양하게 만들 수 있습니다.



## iter()

파이썬 내장 메소드의 `iter`와 `next`는 각각  `__iter__`와 `__next__`메소드를 호출해줍니다. 

### 기본문법

`iter`메소드는 설정하는 **인자값의 개수에 따라 첫번째 인자값의 해석이 달라집니다.** 

 - 하나의 인자값만 설정할 경우에는 반복가능한 객체를 설정
   	- `iter(반복가능한 객체)`


   	
 - 두개의 인자값을 설정할 경우엔 첫번째 인자값으로 `callable객체`를, 두번째 인자값에는 반복을 끝낼 값을 설정해야 합니다.  
   [👉callable 객체에 대한 설명 보러가기 ](https://2sehi.github.io/python/50_Python_Basic/)
   - `iter(callable 객체, 반복을 끝낼 값)`



### 예시

다음과 같이 `range(3)`은 `callable객체`가 아닌데 반복을 끝낼 값이 없을땐 돌아가고  반복을 끝낼 값이 있을 땐 `range(3)`가 `callable객체`가 아니라면서 에러가 발생합니다.


~~~python
print(callable(range(3)))

# 출력 결과
```
False
```
~~~

   

- 인자값이 1개인 경우 , range(3)는 `__iter__`메소드를 가진 반복가능한 객체이므로 다음과 같이 `iterator객체`를 생성하고 요소를 하나씩 꺼내는 것이 가능합니다.

~~~python
it = iter(range(3))
print(next(it))  # 0
print(next(it)) # 1
print(next(it)) # 2
print(next(it)) # error

# 출력 결과
```
0
1
2
Traceback (most recent call last):

  File "C:\Users\krseh\.spyder-py3\temp.py", line 5, in <module>
    print(next(it)) # error

StopIteration
```
~~~





- 인자값이 2개인 경우 , range(3)는 `callable객체`가 아니므로 에러가 발생합니다.

````python


it = iter(range(5), 3) # TypeError: iter(v, w): v must be callable
print(next(it))
print(next(it))
print(next(it))
print(next(it))

# 출력 결과
```
Traceback (most recent call last):

  File "C:\Users\krseh\.spyder-py3\temp.py", line 1, in <module>
    it = iter(range(3),2)

TypeError: iter(v, w): v must be callable
        `
```
````



#### for문에 사용

- 다음과 같이 `for문`에 `iter(callable객체, 반복을 끝낼 값)`을 반복하도록 하면 `next()`메소드를 직접 호출하지 않아도 되고 매번 if 조건문으로 2인지 검사하지 않아도 되어 코드가 간결해집니다.

````python
import random
for i in iter(lambda : random.randint(0, 5), 2):
    print(i, end=' ')
    
# 출력 결과
```
0 3 4 0 0 
```
````



## next()

`next()`메소드는 기본값을 지정할 수 있습니다. 기본값을 지정하면 반복이 끝나더라도 StopIteration예외가 발생하지 않고 기본값을 출력하는 것입니다.



### 기본문법

`next(반복가능한 객체, 기본값)`



### 예시

반복이 끝났는데도 예외가 발생하지 않고 `next()`의 두번째 인자에 지정한 기본값이 출력됩니다.

````python
it = iter(range(3))
print(next(it, 10))
print(next(it, 10))
print(next(it, 10))
print(next(it, 10))
print(next(it, 10))

# 출력 결과
```
0
1
2
10
10
```
````



## 심사문제 : 시간 이터레이터 만들기 



[🔻문제 보러가기](https://dojang.io/mod/quiz/view.php?id=2411)

[🔻나의 풀이](https://github.com/2SEHI/Python-Programming-Test/blob/main/python-coding-dojang/unit39-7_iterator.py)
