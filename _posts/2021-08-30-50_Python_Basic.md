---
title:  "[python] callable object(호출가능한 객체)"
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





# callable object

**callable object**란 함수처럼 호출될 수 있는 객체로 **스페셜 메소드 `__call__`을 사용하면 객체를 함수 처럼 사용 할 수 있습니다**.

- 스페셜 메소드 : **특정 상황에서 자동으로 호출 되는 메소드**로 `__init__`메소드도 스페셜 메소드에 해당됩니다.


````python
class PokerPlayer:
    def __init__(self, name):
        self.name = name

    def __call__(self):
        return f"{self.name} wants another card."
    
pocket = PokerPlayer("세희")
print(pocket())

# 출력 결과
```
세희 wants another card.
```
````


# callable()

`callable()`메소드를 이용하여 callable object인지 아닌지를 확인할 수 있는데,  callable object이면 `True`를 반환하고 그렇지 않으면 `False`를 반환합니다. 

````python
callable(range(3))

# 출력 결과
```
False
```
````

[참고 링크] : [https://medium.com/swlh/callables-in-python-how-to-make-custom-instance-objects-callable-too-516d6eaf0c8d](https://medium.com/swlh/callables-in-python-how-to-make-custom-instance-objects-callable-too-516d6eaf0c8d)

