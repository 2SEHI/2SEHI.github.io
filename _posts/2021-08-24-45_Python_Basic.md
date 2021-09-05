---
title:  "[python] getter/ setter와 property함수"
excerpt: ""

toc: true
toc_sticky: true

date: 2021-08-24
last_modified_at: 2021-08-24

use_math: true
comments: true

categories:

  - Python

---

_"파이썬\_코딩도장"을 읽고 정리한 내용입니다._



## getter 와 setter

[지난 포스팅](https://2sehi.github.io/python/40_Python_Basic/#%EB%B9%84%EA%B3%B5%EA%B0%9C-%EC%86%8D%EC%84%B1)에서 인스턴스로부터 속성에 직접 접근하지 못하도록 속성의 앞에 \__를  붙여주는 비공개 속성에 대해서 알아보았습니다. 

비공개 속성을 활용하여 getter/setter라는 메소드를 만들어 볼 건데 클래스 인스턴스의 내부 데이터를 보호하기 위해서 인스턴스에서 직접 데이터에 접근하지 못하도록 하는 방법으로 자바나 다른 언어에서도 사용합니다. 입력값을 받아오거나 데이터베이스에서 select결과를 가져올 때 등 getter/setter를 이용하여 값을 주고받습니다.

값을 가져오는 것을 getter, 값을 설정하는 것을 setter라고 하는데 왜 속성명을 그대로 사용하면 안될까요? 


- 먼저 첫번째 이유는 한 인스턴스의 내부속성을 외부에서 알지 못하게 하고, 접근이 불가능하게 하여 객체지향의 목적인 `정보은닉`을 보장하기 위해서입니다.다.
- 두번째는 call by value가 아닌 call by reference를 통해 `무결성 보장`을 위해서 입니다.


학생의 이름, 국어점수, 영어점수, 수학점수를 저장하는 클래스를 생성한다고 합시다.

```python
class Student:
    def __init__(self, name, kor, eng, math):
        self.name = name
        self.kor = kor
        self.eng = eng
        self.math = math
```



그리고 Student의 인스턴스를 생성하여 학생정보를 설정해보겠습니다.

````python
student = Student('파이썬', -100, 90, 89)
print(student.kor)

# 출력 결과
```
-100
```
````



- 보통 0점 이하의 점수는 있을 수 없습니다. 이를 settter를 이용해서 0점과 100점사이의 점수로만 저장할 수 있게 할 수 있습니다.

먼저 속성 앞에 __를 붙여서 직접 접근하지 못하도록 하고 기본값을 초기화함수 내부에서 설정해줍니다.

```python
class Student:
    def __init__(self):
        self.__name = 'noname'
        self.__kor = 0
        self.__eng = 0
        self.__math = 0

```



- setter함수의 이름을 set_kor(본인마음대로 지정가능)라고 지정하고 인자값으로 국어점수를 받습니다.  그리고 점수를 인자값으로 받은 값으로 저장하고 0보다 작거나 100보다 크면 이상치이므로 0으로 변경합니다.

```python
    def set_kor(self, kor):
        self.__kor = kor
        if kor < 0 or kor > 100: 
            self.__kor = 0
```



- 인스턴스를 생성하여  set_kor함수를 이용해 점수를 설정해줍니다.

```python
student = Student()
student.set_kor(-100)
```



- 이제 값을 설정했으니 가져오는 getter를 만들어줍니다. getter는 reutrn으로 값을 가져오도록 만들면 됩니다.

```python
    def get_kor(self):
        return self.__kor
```



- student인스턴스를 이용해 set_kor(-100)을 설정하고 get_kor()함수를 호출하면 0점이 출력됩니다.

```python
class Student:
    def __init__(self):
        self.__name = 'noname'
        self.__kor = 0
        self.__eng = 0
        self.__math = 0

    def set_kor(self, kor):
        if kor < 0 or kor > 100: 
            self.__kor = 0
        else :
            self.__kor = kor
            
    def get_kor(self):
        return self.__kor

student = Student()
student.set_kor(-100)
student.get_kor()
```





다른 속성에 대해서도 같은 방식으로 원하는 조건을 추가하여 setter/getter함수를 만들어주면 됩니다.



## property()

파이썬의 내장 함수인 property()를 사용하면 마치 필드명을 사용하는 것처럼 깔끔하게 getter/setter 메서드가 호출되게 할 수 있습니다.

`사용할 속성명 = property(getter함수, setter함수)`로 만들어줍니다.

```python
    # 프로퍼티 생성
    # 변수처럼 사용하는데 kor= 은 하게되면 setter가 호출되고 
    # kor하게 되면 getter가 호출됩니다.
    name = property(get_name, set_name)
    kor = property(get_kor, set_kor)
    eng = property(get_eng, set_eng)
    math = property(get_math, set_math)
```



그리고 위에서 설정한 속성명에 인스턴스로 접근하여 값을 설정하고 가져오면 됩니다.

```python
# 설정
p1.name = '파이썬'

# 값 가져오기
print(p1.name)
```

