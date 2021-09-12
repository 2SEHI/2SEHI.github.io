---
title:  "[python] closure"
excerpt: "변수의 사용범위와 closure를 만드는 방법에 대해 정리했습니다."

toc: true
toc_sticky: true

date: 2021-08-23
last_modified_at: 2021-08-23

use_math: true
comments: true

categories:
  - Python
---

_"파이썬\_코딩도장"을 읽고 정리한 내용입니다._





# closure

closure에 대해 알아보기 앞서 변수의 사용범위인 전역변수와 지역 변수에 대해 알아보겠습니다.



## 1.전역변수과 지역변수
### 1) 전역변수 (Global Variable)

- 함수를 포함하여 스크립트 전체에 접근할 수 있는 변수입니다.



````python
# 전역 변수 선언
x = 10
def foo():

    print('전역변수 x의 값이 출력됩니다 : ', x)

foo()
print('전역변수 x의 값이 출력됩니다 : ', x)
# 출력 결과
```
전역변수 x의 값이 출력됩니다 :  10
전역변수 x의 값이 출력됩니다 :  10
```
````



### 2) 지역변수(Local Variable)

- 지역변수는 변수를 만든 함수안에서만 접근할 수 있고, 함수 바깥에서는 접근할 수 없습니다.
- 지역변수는 접근 가능 범위의 처리가 끝나면 메모리가 자동 소멸됩니다.

````python
def foo():
    # 지역 변수 선언
    y = 10
    print('지역변수 x의 값이 출력됩니다 : ', y)

foo()
print('지역변수의 x의 값은 함수 밖에서 출력할 수 없습니다', y)

# 출력 결과
```
지역변수 x의 값이 출력됩니다 :  10
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-3-219ec3693543> in <module>()
      5 
      6 foo()
----> 7 print('지역변수의 x의 값은 함수 밖에서 출력할 수 없습니다', y)

NameError: name 'y' is not defined
```
````





### 3) global 전역변수

- 함수안에서 x의 값을 변경한 것처럼 보이지만 이름만 같을 뿐 별개의 변수입니다. 

````python
# 전역변수 선언
x = 10
def foo():
    # 지역변수 x 선언
    x = 6
    print('지역변수 x의 값이 출력됩니다 : ', x)

foo()

# 함수안에서 x의 값을 변경한 것처럼 보이지만 
# 이름만 같을 뿐 별개의 변수입니다. 
print('전역변수 x의 값이 출력됩니다 : ', x)

# 출력 결과
```
전역변수 x의 값이 출력됩니다 :  6
전역변수 x의 값이 출력됩니다 :  10
```
````





- 함수 안에서 전역 변수의 값을 변경하려면 `global`  키워드를 사용해야 합니다.

````python
# 전역변수 선언
x = 10
def foo():
    # 전역변수 x를 사용하겠다고 설정
    global x
    x = 6
    print('전역변수 x의 값이 출력됩니다 : ', x)

foo()

# 함수안에서 x의 값을 변경한 것처럼 보이지만 
# 이름만 같을 뿐 별개의 변수입니다. 
print('전역변수 x의 값이 출력됩니다 : ', x)

# 출력 결과
```
전역변수 x의 값이 출력됩니다 :  6
전역변수 x의 값이 출력됩니다 :  6
```
````



### 4) for 또는 if와 전역변수

- 파이썬에서 제어문과 반복문은 블럭이 아니어서 제어문이나 반복문 안에서 선언된 변수는 전역변수로 밖에서도 사용이 가능합니다
- 자바에서는 제어문을 스택으로 만들기 때문에 제어문안에서 선언한 변수를 밖에서 사용할 수 없습니다.

````python
if True :
    # 전역변수 x 선언
    x = 0

# 전역변수 x 출력
print('if문 밖에서 전역변수 x의 값이 출력됩니다 : ', x)

# 출력 결과
```
if문 밖에서 전역변수 x의 값이 출력됩니다 :  0
```
````



### 5) 함수안의 함수

함수 안에서 함수를 선언할 수 있습니다. 그리고 함수와 클래스는 한번 만들어지면 메모리에 계속 유지되는데 while문이나 for문 안에서 함수를 선언하면 반복문이 끝나고 함수가 메모리에 남지 않게 됩니다.

```python
def 함수이름1():
	코드
	def 함수이름2():
		코드
```

````python
def print_hello():
    hello = 'Hello, world'
    # 함수안의 함수 선언
    def print_message():
        print(hello)
        
    # 함수안의 함수 호출
    print_message()
    
print_hello()

# 출력 결과
```
Hello, world
```
````



### 6) nonlocal

함수A에서 선언된 x를  함수 B에서 변경하려고 하는데 함수B안에서 `x=20`이라고 변경한 것같지만 사실은 B에서 새로 선언된 것입니다. 

````python
def A():
    # A함수의 지역변수 x
    x = 10
    def B():
        # B함수의 지역변수 x
        x = 20
    B()
    
    print('함수 A에서 선언된 지역변수 x가 변경되지 않고 선언됩니다.',x)
A()

# 출력 결과
```
함수 A에서 선언된 x가 선언됩니다. 10
```
````



`A`함수의 지역변수 x를 `B`함수에서 변경하려면  `nonlocal `이라고 선언해주면 됩니다.

````python
def A():
    # A함수의 지역변수 x
    x = 10
    def B():
        nonlocal x
        # A함수의 지역변수 x
        x = 20
    B()
    
    print('함수 A에서 선언된 지역변수 x가 함수 B에서 변경되어 선언됩니다.',x)
A()

# 출력 결과
```
함수 A에서 선언된 x가 선언됩니다. 20
```
````



## 2.closure



파이썬에서는 함수를 둘러싼 환경을 계속 유지하다가, 함수를 호출할 때 다시 꺼내서 사용하는 함수를 closure라고 합니다. 

javascript, Kotlin, Python, Swift와 같은 스크립트 언어에 존재하는 함수 사용 기법으로 함수 내부의 데이터를 함수 외부에서 수정할 수 있도록 만드는 함수를  closure라고 합니다. Swift에서는 closure를 만들 수 있는데 공식 문서에서는 람다를 closure라고 표현합니다.

추가적으로 clousre라는 프로그래밍 언어도 있는데, 미국에서는 종종 서버용 언어로 사용합니다.



파이썬에서의 closure는 지역 변수와 코드를 묶어서 사용하고 싶을 때 활용하며, closure에 속한 지역 변수는 바깥에서 직접 접근할 수 없으므로 데이터를 숨기고 싶을 때 활용합니다. 



### 1) closure만들기

- 이때 주의할 점은 함수를 반환할 때 함수 이름으로 반환하며, 괄호를 붙이면 안됩니다.

````python
def calc():
    a = 3
    b = 5
    def mul_add(x):

        return a * x + b
        # 함수를 반환할 때는 함수 이름만 반환하며, 괄호를 붙이면 안됩니다.
    return mul_add

c = calc()
print(c(1), c(2), c(3))

# 출력 결과
```
8 11 14
```
````



### 2) lambda를 활용한 closure

closure는 lambda와 자주 사용됩니다. 위에서 ```mul_add```함수를  lambda표현식으로 바꾸어 주면 됩니다.

````python
def calc():
    a = 3
    b = 5
    
	return lambda x : a*x + b

c = calc()
print(c(1), c(2), c(3))

# 출력 결과
```
8 11 14
```
````



### 3) closure를 활용한 지역변수 변경

```python
def calc():
    a = 3
    b = 5
    total = 0
    def mul_add(x):
        # calc 함수의 지역변수 total을 사용하도록 함
        nonlocal total
        # total 저장
        total = total + a * x + b
        # total 반환
        return total
        # 함수를 반환할 때는 함수 이름만 반환하며, 괄호를 붙이면 안됩니다.
    return mul_add

c = calc()
print(c(1), c(2), c(3))
```











