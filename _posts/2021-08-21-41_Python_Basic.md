---

title:  "[python] 클래스 속성"
excerpt: "클래스 속성, 정적 메소드에 대해 공부했습니다"

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





## 인스턴스 속성과 클래스 속성

지난 포스팅에서 ```__init__```메소드에서 만들어진 속성을 인스턴스 속성이라고 배웠습니다. 

클래스 속성은 클래스에서 바로 만드는 속성으로 모든 인스턴스에 공유되는 속성입니다.





선언은 다음과 같이 메소드가 아닌 클래스에 바로 선언해 주면 됩니다.

 ```python
 class Person:
     bag = []
 ```



호출은 어떻게 할까요? 메소드 바깥에 선언된 클래스 속성은 다른 인스턴스끼리 공유 된다고 했습니다.   그리고 메소드 내에서 선언된 self는 인스턴스입니다. 인스턴스 속성이 아닌 클래스 속성임을 명시하기 위해 메소드에서 클래스속성을 사용할 때는 ```클래스.속성```으로 접근해야합니다.



james가 bag에 "책"을 넣고, maria는 bag에 "지갑"을 넣었는데 bag이 클래스 속성이어서 모든 인스턴스가 공유하여 james의 bag에서 maria가 넣은 "지갑"이 출력됩니다.

````python
class Person:
    bag = []
    def put_bag(self, stuff):
        Person.bag.append(stuff)

    def print_bag(self):
        print(Person.bag)

james = Person()
james.put_bag('책')

maria = Person()
maria.put_bag('지갑')

james.print_bag()

# 출력 결과
```
['책', '지갑']
```
````





클래스와 인스턴스 각각의 속성을 ```__dict__```로 확인해보면 bag은 클래스에 속해 있는 것을 확인할 수 있습니다.

````python
Person.__dict__

# 출력 결과
```
mappingproxy({'__dict__': <attribute '__dict__' of 'Person' objects>,
              '__doc__': None,
              '__module__': '__main__',
              '__weakref__': <attribute '__weakref__' of 'Person' objects>,
              'bag': ['책', '지갑'],
              'print_bag': <function __main__.Person.print_bag>,
              'put_bag': <function __main__.Person.put_bag>})
```
````



````python
james.__dict__
# 출력 결과
```
{}
```
````





## 비공개 클래스 속성

클래스를 비공개로 만드는 방법은 속성이나 메소드와 같이 앞에 밑줄 두개를 붙이면 됩니다. 

```python
def 클래스:
	__속성 = 값
```





````python
class Knight:
    # 공개 클래스 속성
    bag = []
    # 비공개 클래스 속성
    __item_limit = 10

    def print_item_limit(self):
        print(Knight.__item_limit)

x = Knight()
x.print_item_limit()

print(Knight.__item_limit)

# 출력 결과
```
10
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-37-d24756e79aff> in <module>()
      9 x.print_item_limit()
     10 
---> 11 print(Knight.__item_limit)

AttributeError: type object 'Knight' has no attribute '__item_limit'
```
````





그러나  ```__dict__```로 클래스의 속성을 확인해보면 bag뿐만 아니라 ```__item_limit```와 그의 값까지 보여집니다.

````python
Knight.__dict__

# 출력 결과
```
mappingproxy({'_Knight__bag': [],
              '_Knight__item_limit': 10,
              '__dict__': <attribute '__dict__' of 'Knight' objects>,
              '__doc__': None,
              '__module__': '__main__',
              '__weakref__': <attribute '__weakref__' of 'Knight' objects>,
              'print_item_limit': <function __main__.Knight.print_item_limit>})
```
````





## 클래스에서 메소드 직접 호출





### 정적 메소드

정적 메소드는 인스턴스를 생성하여 인스턴스로 메소드를 호출하지 않아도 클래스에서 바로 호출할 수 있습니다. 

```python
class 클래스이름:
    @staticmethod
    def 메소드(매개변수1, 매개변수2):
        실행코드
```





다음과 같이 정적 메소드를  클래스에서 직접 호출하여 접근이 가능합니다. 보통은 인스턴스의 상태를 변화시키지 않는 메소드를 만들 때 사용합니다.

```python
class Calc:
    @staticmethod
    def add(a, b):
        print(a + b)
	@staticmethod
    def mul(a, b):
        print(a * b)

Calc.add(3, 5)
Calc.mul(3, 5)
```





### 클래스 메소드

클래스 메소드는 클래스에서 직접 접근 가능한 메소드입니다. 클래스 메소드는 메소드 위에 ```@classmethod```를 붙이고 메소드의 첫번째 매개변수에 cls를 지정해야 합니다.

```python
class 클래스:
    속성 = 값
    @classmethod
    def 메소드(cls, 매개변수1, 매개변수2):
        cls.속성 # cls로 클래스 속성에 접근
```





```greeting```은 클래스에서 직접 호출이 불가능하지만 ```@classmethod```라고 명시해준 ```print_count```메소드는 클래스에서 직접 호출이 가능합니다.

````python
class Person:
    count = 0
    def __init__(self, hello):
        self.hello = hello
        Person.count += 1

    def greeting(self):
        self.hello = '안녕하세요'

    @classmethod
    def print_count(cls):
        print('{0}명 생성 되었습니다.'.format(cls.count))

# 클래스 메소드이므로 클래스에서 직접 접근 가능
Person.print_count()
# 인스턴스 메소드이므로 클래스에서 직접 접근 불가
Person.greeting()

# 출력 결과
```
0명 생성 되었습니다.
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-67-98ddac81e4f5> in <module>()
     13 
     14 Person.print_count()
---> 15 Person.greeting()

TypeError: greeting() missing 1 required positional argument: 'self'
```
````





cls를 사용하면 메소드 안에서 현재 클래스의 인스턴스를 만들 수도 있습니다. ```cls()```는 클래스이므로 ```cls()```는 ```Person()```과 같습니다.

````python
class Person:
    def __init__(self, name):
        self.name = name

    @classmethod
    def create(cls, name):
        p = cls(name)
        return p

james = Person.create("제임스")
james.name

# 출력 결과
```
'제임스'
```
````





```@staticmethod```와 ```@classmethod```를 이용하여 시간을 입력받아 시간의 형식을 확인하는 처리를 만들었습니다. 

```python
class Time :
    def __init__(self, hour, minute, second):
        self.hour = hour
        self.minute = minute
        self.second = second

    @classmethod
    def from_string(cls, time):
        h, m, s = map(int, time.split(':'))
        t = cls(h, m, s)
        return t

    @staticmethod
    def is_time_valid(time):
        h, m, s = map(int, time.split(':'))
        if h > 24 or m > 59 or s > 60:
            return False
        return True
        
time_string = input()

if Time.is_time_valid(time_string):
    t = Time.from_string(time_string)
    print(t.hour, t.minute, t.second)
else :
    print("잘못된 시간 형식입니다.")
```



