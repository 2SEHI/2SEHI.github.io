---
title:  "[python] 문자열에서 구두점을 제거하는 방법"
excerpt: ""

toc: true
toc_sticky: true

date: 2021-09-17
last_modified_at: 2021-09-17

use_math: true
comments: true

categories:
  - Python
---



# 구두점 제거하는 방법

유효한 팰린드롬 를 공부하다가 문자열에서 구두점을 제거하는 여러가지 방법에 대해 정리 해보았습니다.



```python
sentence = "A man, a plan, a canal: Panama"
```



## 1) 정규식 표현 이용

```python
re.sub(r"[^\w]", "", sentence)
```

AmanaplanacanalPanama



## 2) maketrans(a, b) 이용

`string.punctuation`를 출력해보면 `!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~`의 구두점이 들어있는데 문자열에서 문자를 하나씩 추출하며 구두점이 아닌 문자만 out 변수에 저장합니다.

```python
import string
out = ''.join([i for i in sentence if i not in string.punctuation])
print(out)
```

A man a plan a canal Panama





## 3) replace(t, "") 이용

이 방법 또한 `string.punctuation`를 이용하는데 구두점문자열을 하나씩 순회하며 문자열에 포함된 구두점은 공백으로 replace해주는 방법입니다.

```python
temp = string.punctuation
for t in temp:
    sentence = sentence.replace(t, "")
print(sentence)
```

A man a plan a canal Panama



## 4) isalnum() 이용

`isalnum()` 은 문자가 영문자 또는 숫자인 경우 True를 반환해줍니다. 이를 이용하여 영문자 또는 숫자인 경우에만 문자열을 저장합니다.

```python
out = ''.join([s for s in sentence if s.isalnum()])
print(out)
```

AmanaplanacanalPanama