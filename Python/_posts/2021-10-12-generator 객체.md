### 1. 개요

\- 함수 중간중간에 계산된 결과값으로 이루어진 iterable한 객체를 만들고 싶을 때, yield 예약어를 사용하면 그 결과값으로 이루어진 iterable한 객체인 generator 객체를 만들 수 있다. 이렇게 만들어진 generator 객체는 그 자체로도 sum() 함수 등의 인자가 되기도 하고 list() 등의 함수로 형변환하여 사용할 수도 있다.

\- 구체적으로, generator 객체는 다음과 같은 방식으로 만들 수 있다.

```python
def makeGenObj(param1):
    for i in range(param1):
        yield i

genObj = makeGenObj(10)
```


### 2. for in 구문으로 generator 만들기(generator expression)

#### 1) for in 구문

```python
def func1(param):
    return param**2

gen1 = (i**2 for i in range(5))
gen2 = (func1(i) for i in range(5))
gen3 = ([i, i**2] for i in range(5))
```

\- 괄호쌍 안에 **연산식 + for in 구문**을 써서 generator 객체를 1줄의 구문으로 선언할 수 있다. 위 코드와 같이, 객체의 원소를 받는 변수(위 식의 i)에 관한 연산식을 for in 구문 앞에 쓰면 for in 구문으로 인해 증가하는 i값이 연산식에 대입되어 그 리턴값들로 이루어진 generator 객체가 생성된다.

\- for in 구문 앞의 연산식을 함수 호출로 대신할 수도 있다. 위 코드의 경우 gen1과 gen2의 결과값이 모두 (0, 1, 4, 9, 16)으로 동일하다.

\- 연산식 대신 리스트나 튜플 같은 객체의 형태를 쓰는 것도 가능하다. 예를 들어 위 코드의 gen3은 ([0, 0], [1, 1], [2, 4], [3, 9], [4, 16])이 저장된다.


#### 2) for in 구문 뒤에 if 구문 추가된 형태

```python
gen1 = (i for i in range(10) if i%2 == 0)
```

\- for in 구문 뒤에 **if 구문을 추가한 형태**를 쓸 수 있다. 이 경우 각 루프마다 if 뒤의 조건식이 참이 되는지 여부를 확인하고, 참일 때에만 그때에 해당하는 원소를 for in 구문 앞의 연산식에 대입해 이러한 연산식의 결과값으로 구성된 generator 객체를 생성한다.


#### 3) for in 구문이 중첩된 형태

```python
gen1 = ([i, j] for i in range(2) for j in range(3))
```

\- for in 구문은 중첩해 쓸 수 있으며, 이 경우 앞의 for in 구문의 매 루프마다 뒤의 for in 구문의 루프가 수행되는 식으로 연산이 일어난다. 예를 들어 위 코드의 gen1은 ([0, 0], [0, 1], [0, 1], [1, 0], [1, 1], [1, 2])가 저장된다.