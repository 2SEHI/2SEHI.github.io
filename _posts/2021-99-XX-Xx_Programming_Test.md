## is 와 ==

is는 id 값을 비교하는 함수이며 ==는 값을 비교하는 연산자입니다.

```python
a = [1, 2, 3]
# 얕은 복사
b = a.copy()
# 깊은 복사
c = a
print('a와 b는 id가 다릅니다 : ', a is b)
print('a와 b는 값이 같습니다 : ', a == b)
print('a와 c는 id가 같습니다 : ', a is c)
```



## locals()

- 클래스 메소드 내부의 모든 로컬 변수를 출력해 변수명을 일일이 찾아낼 필요 없이 잘못 선언된 부분이 없는지 확인하는 용도로 활용할 수 있습니다.



## isalnum()

- 문자열이 알파벳([a-zA-Z])과 숫자([0-9])로만 구성되었는지 확인하는 파이썬 문자열 메소드입니다.



### collections.deque()

- stack과 queue를 변형한 형태로 양쪽끝에서 추가(append)와 삭제(pop)가 가능합니다.

https://docs.python.org/3/library/collections.html#collections.deque

- `append`(*x*) : deque의 오른쪽에 *x*를 추가합니다.

- `appendleft`(*x*) : 데크의 왼쪽에 *x*를 추가합니다.

- `count`(*x*) : *x* 와 같은 데크 요소의 수를 셉니다.

- `insert`(*i*, *x*) : *x*를 데크의 *i* 위치에 삽입합니다.

- `pop`() : 데크의 오른쪽에서 요소를 제거하고 반환합니다. 요소가 없으면, [`IndexError`](https://docs.python.org/ko/3/library/exceptions.html#IndexError)를 발생시킵니다.

- `popleft`() : 데크의 왼쪽에서 요소를 제거하고 반환합니다. 요소가 없으면, [`IndexError`](https://docs.python.org/ko/3/library/exceptions.html#IndexError)를 발생시킵니다.

- `maxlen` : 데크의 최대 크기.설정하지 않으면 길이 변경됩니다.



## dictionary



### defaultdict

### OrderedDict

 파이썬 3.6버전이하에서 입력 순서가 유지되도록 한 자료형으로 3.6버전 이상부터는 일반 Dictionary에서도 입력 순서가 유지되어 OrderedDict는 사용할 필요가 없습니다. 하지만 코딩 테스트에서 하위버전의 파이썬 인터프리터를 사용하는 경우가 있으니, 사용방법을 알아두는 것이 좋습니다.
