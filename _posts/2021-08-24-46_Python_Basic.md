---
title:  "[python] for ... else ... 문"
excerpt: "파이썬의 독특한 문법 for ... else .. 문에 대해 정리해보았습니다."

toc: true
toc_sticky: true

date: 2021-08-24
last_modified_at: 2021-08-24

use_math: true
comments: true

categories:
  - Python

---



# for ... else ...

보통 `else`는  `if문`과 사용되며 `if문`의 조건이 맞지 않을 경우 실행될 처리를 구현하는데 사용됩니다. 



또한 일반인 `for문`의 경우 `for문`안에서 내용을 수행하다가 break를 만나면 반복문이 중단됩니다. 



그런데 아래와 같이 `for문`과 `else문`이 동등한 indent의 위치에 있는 경우 `for문`안의 구문을 `break`로 중단하지 않으면 `else문`에 있는 `내용3`을 실행하게 됩니다 .

```python
for  반복구문:
    내용1
    if 조건문:
        내용2
        break
else: 
    내용3
```

for문이 한번만 수행된다는 가정하에 처리 순서는 아래와 같습니다.




1. break를 만나지 않는 경우
    `for 반복구문 => 내용1 => if 조건문` 
2. break를 만나는 경우
    `for 반복구문 => 내용1 => if 조건문 => 내용2 => break => else: 내용3` 

- 이는 for 대신에 while문에도 사용할 수도 있습니다.