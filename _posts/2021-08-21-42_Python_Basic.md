---

title:  "[python] 클래스 상속"
excerpt: "상속 및 오버라이딩에 대해 알아보았습니다."

toc: true
toc_sticky: true

date: 2021-08-21
last_modified_at: 2021-08-21

use_math: true
comments: true

categories:
  - Python
---

_"파이썬\_코딩도장"을 읽고 정리한 내용입니다._





# 클래스 상속

클래스 상속은 기반 클래스의 기능을 유지하면서 파생 클래스를 이용하여 새로운 기능을 추가할 수 있습니다.





## 기반 크래스와 파생 클래스

- 기반클래스 : 기능을 물려주는 클래스, 

- 파생 클래스 : 상속을 받아 새롭게 만드는 클래스



클래스를 상속하는 방법은 파생클래스의 매개변수에 기반클래스이름을 지정해주면 됩니다.

```python
class 기반클래스이름:
    코드

class 파생클래스이름(기반클래스이름):
    코드
```





## 기반클래스의 메소드 호출

클래스를 상속하면 파생클래스의 인스턴스를 생성하여 기반클래스의 메소드를 호출할 수 있습니다.

```python
# 기반 클래스
class Person:
    def greeting(self):
        print('안녕하세요')

# 파생 클래스
class Student(Person):
    def study(self):
        print('영어공부하기')

# 파생클래스의 인스턴스 생성
james = Student()
# 파생클래스의 인스턴스를 이용하여 기반클래스의 greeting메소드 호출
james.greeting()
# 파생클래스의 인스턴스를 이용하여 파생클래스의 study메소드 호출
james.study()

```





## issubclass(파생클래스, 기반클래스)

클래스의 상속 관계를 확인하고 싶을 때 ```issubclass```함수를 이용하면 됩니다. 첫번째 인자에 파생클래스, 두번째 인자에 기반클래스를 설정하여 출력하면 매개변수에 설정한 파생클래스와 기반클래스의 관계가 맞다면 True, 아니면 False를 반환합니다.

````python
issubclass(Student, Person)

# 출력 결과
```
True
```
````





## super().\__init__()

파생클래스의 인스턴스로부터 기반 클래스의 속성을 호출할 때는 ```super()```함수를 이용하여 기반 클래스의 ```__init__```함수를 호출하여 기반 클래스를 초기화해주어야 속성 호출이 가능합니다.

````python
# 기반 클래스
class Person:
    def __init__(self):
        self.hello = "안녕하쇼"

# 파생 클래스
class Student(Person):
    # __init__함수 생략
    def study(self):
        print("영어공부하기")

# 파생클래스의 인스턴스 생성
james = Student()
# 파생클래스를 이용하여 기반 클래스의 속성 호출
print(james.hello)

# 출력 결과
```
안녕하쇼
```
````





## super().\__init__()을 안해도 되는 경우

파생클래스의 ```__init__```함수가 생략되어 있으면 자동으로 기반 클래스의 ```__init__```함수가 호출되므로 ```super().__init__()```으로 기반 클래스를 초기화하지 않아도 됩니다.



````python
# 기반 클래스
class Person:
    # 초기화함수
    def __init__(self):
        self.hello = "안녕하쇼"

# 파생 클래스
class Student(Person):
    def study(self):
        print("영어공부하기")

# 파생클래스의 인스턴스 생성
james = Student()
# 기반 클래스의 속성 hello 호출
print(james.hello)

# 출력 결과
```
안녕하쇼
```
````





## super(파생클래스, self) 명확하게 사용하기

```super().__init__```으로 기반 클래스를 초기화할 때 첫번째 인자에는 파생클래스를, 두번째 인자에는 self를 지정하여 현재 클래스가 어떤 클래스인지 명확하게 표시하는 것이 좋습니다.

```python
class Student(Person):
    def __init__(self)L
    print('Student __init__')
    super(Student, self).__init__()
    self.school = '와플대학'
```





## 메소드 오버라이딩

기반 클래스에 있는 메소드를 파생 클래스에서 재정의 하는 것을 메소드 오버라이딩이라고 합니다.

````python
# 기반 클래스
class Hello:
    def greeting(self):
        print('hello')

# 파생 클래스
class Korean(Hello):
    # 메소드 재정의
    def greeting(self):
        print("안녕하세요")

hello = Korean()
# 파생클래스의 메소드 호출
hello.greeting()

# 출력 결과
```
안녕하세요
```
````





파생클래스에서 메소드를 재정의하려는데 기반 클래스의 메소드와 중복되는 부분이 있어서 일부 사용하고 싶을 때는 ```super().메소드()```로 호출하여 중복을 줄일 수 있습니다.

````python
 # 기반 클래스
 class Person : 
     def greeting(self):
         print('안녕하세요')

# 파생 클래스
class Student(Person):
    # 메소드 재정의
    def greeting(self):
        # 기반 클래스의 메소드 호출
        super().greeting()
        print('저는 이세희입니다.')

james = Student()
# 파생 클래스의 메소드 호출
james.greeting()

# 출력 결과
```
안녕하세요
저는 이세희입니다.
```
````





## 다중 상속

지금까지는 하나의 기반 클래스로부터 상속받는 단일 상속이었지만, 다중 상속은 여러 기반 클래스로부터 상속을 받아서 파생 클래스를 만드는 방법입니다. 

```python
class 기반클래스이름1:
    코드

class 기반클래스이름2:
    코드

class 파생클래스이름(기반클래스이름1, 기반클래스이름2):
    코드

```





 아래는 두개의 기반클래스를 상속받은 경우입니다.

````python
# 기반클래스1
class Person:
    def greeting(self):
        print('안녕하세요')

# 기반클래스2
class University:
    def manage_credit(self):
        print("학점관리")

# 파생클래스(기반클래스1, 기반클래스2)
class Undergraduate(Person, University):
    def study(self):
        print("공부하기")

james = Undergraduate()
james.greeting()
james.manage_credit()
james.study()

# 출력 결과
```
안녕하세요
학점관리
공부하기
```
````





만약에 두개의 기반 클래스에 같은 메소드가 있다면 파생클래스는 클래스 생성시 지정한 첫번째 인자의 기반클래스1을 먼저 호출합니다.

````python
# 기반클래스1
class Person:
    def study(self):
        print('안녕하세요')

# 기반클래스2
class University:
    def study(self):
        print("학점관리")

# 파생클래스(기반클래스1, 기반클래스2)
class Undergraduate(Person, University):
    def studying(self):
        print("공부하기")

james = Undergraduate()
james.study()

# 출력 결과
```
안녕하세요
```
````





## mro()

```클래스.mro()```를 이용하여 다중 상속의 메소드 탐색(호출) 순서를 확인할 수 있습니다.

````python
Undergraduate.mro()

# 출력 결과
```
[__main__.Undergraduate, __main__.Person, __main__.University, object]
```
````

