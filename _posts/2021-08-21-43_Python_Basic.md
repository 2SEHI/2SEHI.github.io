---

title:  "[python] 추상클래스"
excerpt: "추상클래스에 대해 공부했습니다"

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





# 추상 클래스

추상 클래스는 메소드의 목록만 가지는 클래스로 상속받는 클래스에서 메소드 구현을 강제하기 위해 사용됩니다. 





파이썬에서 추상클래스를 만들려면 abc모듈을 import해야 합니다. 





그리고 추상클래스의 인자에 metaclass=ABCMeta를 지정하고, 메소드위에 @abstractmethod를 붙여서 추상 메소드를 지정합니다. 파생클래스에서 추상클래스를 상속받도록 하여 추상클래스의 추상메소드를 반드시 구현해야 합니다. 구현하지 않으면 에러가 발생하며 파생클래스의 인스턴스를 생성할 수 없습니다.

```python
from abd import *
class 추상클래스이름(metaclass=ABCMeta):
    @abstractmethod
    def 메소드이름(self):
        코드
        
class 파생클래스이름(추상클래스이름):
    def 메소드이름(self):
        코드
```





다음과 같이 추상클래스를 상속받는 파생클래스에서 추상메소드 ```study```메소드는 구현하였지만 ```go_to_school```메소드는 구현하지 않아 에러가 발생합니다.

````python
from abc import *

# 추상클래스 생성
class StudentBase(metaclass=ABCMeta):
    # 추상메소드
    @abstractmethod
    def study(self):
        pass
    # 추상메소드
    @abstractmethod
    def go_to_school(self):
        pass

# 추상클래스를 상속받는 파생클래스 생성
class Student(StudentBase):
    # 추상메소드 구현
    def study(self):
        print('공부하기')

james = Student()
james.study()

# 출력 결과
```
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-114-589e5ebd1c26> in <module>()
     18         print('공부하기')
     19 
---> 20 james = Student()
     21 james.study()
     22 

TypeError: Can't instantiate abstract class Student with abstract methods go_to_school
```
````



### 추상클래스와 인스턴스

추상클래스는 인스턴스로 만들 수 없는데 파생클래스에서 받드시 구현해야 할 메소드를 명시하기 위해서 사용되며 추상클래스의 인스턴스는 생성될 일이 없어 추상메소드안에는 ```pass```만 넣음으로써 비어있는 메소드로 만듭니다.

````python
james = StudentBase()

# 출력 결과
```
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-115-fcffb19d946f> in <module>()
----> 1 james = StudentBase()

TypeError: Can't instantiate abstract class StudentBase with abstract methods go_to_school, study
```
````





추상 클래스에 추상메소드말고 일반 메소드를 구현하고 파생클래스의 인스턴스로부터 추상클래스의 일반 메소드를 호출하는 것은 가능합니다.

````python
# 추상클래스 생성
class StudentBase(metaclass=ABCMeta):
    # 추상메소드
    @abstractmethod
    def study(self):
        pass

    # 일반메소드
    def go_to_school(self):
        print("go to school by bus")

# 추상클래스를 상속받는 파생클래스 생성
class Student(StudentBase):
    # 추상메소드 구현
    def study(self):
        print("영어공부하기")

james = Student()
# 추상클래스의 메소드 호출
james.go_to_school()

# 출력 결과
```
go to school by bus
```
````

