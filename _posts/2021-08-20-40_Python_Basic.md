---
title:  "[python] 클래스와 인스턴스 속성"
excerpt: "클래스와 인스턴스 속성에 대해 공부했습니다"

toc: true
toc_sticky: true

date: 2021-08-20
last_modified_at: 2021-08-20

use_math: true
comments: true

categories:
  - Python
---

_"파이썬\_코딩도장"을 읽고 정리한 내용입니다._





# 클래스

- 클래스는 객체를 표현하는 문법입니다. 





## 클래스 생성 방법

- 보통 클래스 이름은 대문자로 시작합니다
- 클래스 내 메소드의 첫번째 매개변수는 반드시 ```self```를 지정해야 합니다.

```python
class 클래스이름 :
    def 메소드(self):
        실행 코드
```





아래와 같이 사람 클래스를 작성했습니다.

```python
class Person :
    def greeting(self):
        print('Hi~')
```





### 클래스의 인스턴스 생성

사람 클래스를 사용하기 위해선 인스턴스를 생성해야 합니다

```python
인스턴스명 = 클래스명()
```





그리고 메소드를  호출할 때는 인스턴스를 통해 호출합니다.

```python
인스턴스명.greeting()
```





사람 클래스로부터 ```greeting```메소드를 호출해보면 메소드 내의 출력문이 출력됩니다.

````python
james = Person()
james.greeting()

# 출력 결과
```
Hi~
```
````





```type```메소드를 이용하여 인스턴스가 어떤 클래스인지 확인할 수 있습니다.

````python
print(type(james))

# 출력 결과
```
<class '__main__.Person'>
```
````





## 동일 클래스 안의 메소드 호출

- 동일 클래스 내의 메소드를 호출할 때는 ```self.메소드()``` 형식으로 호출해야 하는데 
  ```self```없이 호출하면 클래스 바깥의 메소드를 호출하게 됩니다.





### 동일 클래스 안의 메소드 호출

````python
class Person :
    def greeting(self):
        print('Hi~')

    def hi(self):
        self.greeting()

def greeting():
    print('Bye!')
    
james = Person()
james.hi()

# 출력 결과
```
Hi~
```
````





### 클래스 바깥의 메소드 호출

````python
class Person :
    def greeting(self):
        print('Hi~')

    def hi(self):
        # Person 클래스 내의 greeting이 아닌 바깥의 greeting호출
        greeting()

def greeting():
    print('Bye!')

james = Person()
james.hi()

# 출력 결과
```
Bye!
```
````







## isinstance(인스턴스, 클래스)

- 첫번째 인자의 인스턴스가 두번째 인자의 클래스인지 확인할 수 있는데 맞으면 True, 아니면 False를 반환합니다.

```python
isinstance(인스턴스, 클래스)
```







### fatorial을 구하는 함수에 활용

````python
# 1부터 입력받은 n까지의 양의 정수를 곱하여 반환하는 함수
def factorial(n):
    # n이 int형이 아니거나 음수일 경우 None반환
    if not isinstance(n, int) or n < 0:
        return None
    # n이 1이면 1을 반환
    if n == 1:
        return 1
    # factorial을 호출하여 n부터 1까지 내려가며 곱하기
    return n * factorial(n-1)

n = 5
result = factorial(n)
print(result)

# 출력 결과
```
120
```
````





## \__init__메소드

- ```__init__```메소드는 인스턴스를 만들 때 호출되는 메소드로 인스턴스를 초기화(initialize)해줍니다.

- 메소드의 앞뒤로 ```__```가 붙은 메소드를 스페셜 메소드 또는 매직 메소드라고 하는데 인스턴스 생성 시 파이썬이 자동으로 호출하는 메소드입니다





## 인스턴스 속성 사용

속성에는 인스턴스 속성과 클래스 속성이 있는데 인스턴스 속성과 클래스 속성의 차이점은 클래스 속성까지 알아본 뒤에 알아보도록 하고  먼저 인스턴스 속성에 대해 알아보겠습니다.

인스턴스 속성이란 인스턴스생성시 인스턴스에 속해지는 속성을 말합니다. 



### \__init__메소드 내에 속성 생성할 경우

- 속성을 만들 때는 ```__init__```메소드 안에서 ```self.속성```에 값을 할당합니다

```python
class 클래스이름:
    def __init__(self):
        self.속성 = 값
```





- 그리고 동일 클래스 내의 다른 메소드에서 속성을 사용하려면 ```self.속성```으로 호출하면 됩니다

````python
class Person :
    def __init__(self):
        self.hello = '안녕'
    def greeting(self):
        print(self.hello)

james = Person()
james.greeting()

# 출력 결과
```
안녕
```
````





### \__init__이외의 메소드에서 속성 생성할 경우

- __init__이외의 메소드에서 생성한 속성 해당 메소드를 호출해야 속성 사용이 가능합니다.

```python
class Person:
    def __init(self):
        self.hi = '안녕'

    def greeting(self):
        self.hello = '안녕하세요'

james = Person()
# __init__메소드 내의 속성은 인스턴스 생성후 바로 사용 가능
james.hi
# greeting메소드 내의 속성은 인스턴스 생성후 바로 사용 불가능
# 에러 발생할 것임
james.hello

# greeting메소드 호출
james.greeting()
# greeting메소드를 호출하고 나서야 
# greeting메소드 내의 속성 사용 가능
james.hello
```





## self

- self는 인스턴스 자기 자신을 의미합니다
- ```__init__```메소드의 self에 들어가는 값은 인스턴스가 생성될 때 self에 클래스 자기 자신이 들어옵니다
- 그리고 인스턴스로부터 메소드가 호출될 때 self에 인스턴스가 들어옵니다





## *args 리스트 언패킹

- 클래스로 인스턴스를 만들 때 리스트를 매개변수에 설정하면 ```*args``` 와 위치 인수를 이용하여 값을 가져올 수 있습니다.

````python
class Person:
    def __init__(self, *args):
        self.name = args[0]
        self.age = args[1]
        self.address = args[2]

    def greeting(self):
        print(f'{self.name}는 {self.address}에 산다')

maria = Person(*['마리아', 20, '서울시 서초구 반포동'])
maria.greeting()

# 출력 결과
```
마리아는 서울시 서초구 반포동에 산다
```
````





## 인스턴스 생성 후 속성 추가

- 인스턴스를 생성한 후에 ```인스턴스.속성 = 값```형식으로 속성을 추가할 수 있습니다.

````python
class Person:
    pass

maria = Person()
maria.name = '마리아'
print(maria.name)

# 출력 결과
```
마리아
```
````





이 때 속성은 인스턴스에만 생성되어 maria가 아닌 다른 인스턴스로 name속성을 호출하면 에러가 발생합니다.



````python
james = Person()
print(james.name)

# 출력 결과
```
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-27-31f99859eaf9> in <module>()
      1 james = Person()
----> 2 print(james.name)

AttributeError: 'Person' object has no attribute 'name'
```
````





## \__slots__

위에서 인스턴스 생성 후 속성을 추가하는 방법을 알아보았는데 ```__slots__```에 사용 가능한 속성 이름을 문자열 리스트로 넣어주면  설정한 속성 이름외에는 추가할 수 없습니다.

또한 ```__slots__``` 에 없는 속성은 ```__init__```메소드에도 추가할 수 없습니다.



예를 들어 다음과 같이 2D포인트인 x, y좌표만을 가지고 뭔가를 하고 싶은데 다른 속성이 추가되는 것을 막기 위해 사용됩니다.

````python
class Points2D:
    # 허용하는 속성리스트
    # 이 속성리스트에 없는 속성은 init에도 추가할 수 없습니다
    __slots__ = ["x", "y"] 

    def __init__(self, x, y):
        self.x = x
        self.y = y

james = Points2D(10, 20)
# __slots__에 없는 속성명 z를 추가하면 에러 발생
james.z

# 출력 결과
```
AttributeError                            Traceback (most recent call last)
<ipython-input-15-8b38547e6d9d> in <module>()
      8 
      9 james = Points2D(10, 20)
---> 10 james.z
     11 
     12 james.__dict__

AttributeError: 'Points2D' object has no attribute 'z'
```
````





## 비공개 속성

비공개 속성은 클래스 바깥에서는 접근할 수 없고 클래스 안에서만 사용할 수 있도록 하는 것입니다. 

비공개 속성은 ```__속성```과 같이 앞에만 밑줄 두개를 붙이면 되는데 다만, ```__속성__```과 같이 뒤에도 밑줄 두개를 붙이면 공개 속성이 됩니다.

```python
class 클래스이름:
    def __init__(self, 매개변수):
        self.__속성 = 값
```



중요한 값인데 바깥에서 함부로 바꾸면 안될 때 비공개 속성을 주로 사용합니다.

````python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.__age = age

james = Person("제임스", 30)
james.__age

# 출력결과
```
AttributeError                            Traceback (most recent call last)
<ipython-input-17-5914d4322ec1> in <module>()
      5 
      6 james = Person("제임스", 30)
----> 7 james.__age

AttributeError: 'Person' object has no attribute '__age'
```
````





## 비공개 메소드

메소드도 메소드명앞에 밑줄 두개를 붙이면 클래스 안에서만 호출가능한 비공개 메소드가 됩니다.

````python
class Person:
    # 비공개 메소드
    def __greeting(self, message):
        print(message)
    
    # 클래스 안에서는 비공개 메소드 호출가능
    def hello(self, message):
        self.__greeting(message)

james = Person()
james.hello("반가워요")

james.__greeting("반가워요")

# 출력 결과
```
반가워요
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-21-fabedda5d087> in <module>()
     11 james.hello("반가워요")
     12 
---> 13 james.__greeting("반가워요")

AttributeError: 'Person' object has no attribute '__greeting'
```
````



