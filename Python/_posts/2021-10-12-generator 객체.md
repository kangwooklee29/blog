### 1. 개요

\- 함수 중간중간에 계산된 결과값으로 이루어진 iterable한 객체를 만들고 싶을 때, yield 예약어를 사용하여 generator 객체를 만들어 사용할 수 있다. 

\- 구체적으로 yield 예약어는 'return과 마찬가지로 함수 호출된 곳으로 값을 반환하되, 반환 후 다시 함수 내부 yield를 호출한 곳으로 돌아와 함수 내부 코드를 계속 수행한다'라는 개념으로 이해하면 정확하다. 예를 들어 아래와 같은 코드를 실행시키면 generator 객체 genObj에는 0부터 9까지의 수가 차례대로 담겨있는 iterable 객체가 담긴다.


```python
def makeGenObj(param1):
    n = 0
    while n < param1:
        yield n
        n += 1

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

\- 괄호쌍 안에 **연산식 + for in 구문**을 써서 generator 객체를 1줄의 구문으로 선언할 수 있다. 위 코드와 같이, 객체의 원소를 받는 변수(위 식의 i)에 관한 연산식을 for in 구문 앞에 쓰면 for in 구문으로 인해 증가하는 i값이 연산식에 대입되어 그 결과값으로 구성되는 generator 객체가 생성된다.

\- for in 구문 앞의 연산식을 함수 호출로 대신할 수도 있다. 위 코드의 경우 gen1과 gen2의 결과값이 모두 (0, 1, 4, 9, 16)으로 동일하다.

\- 연산식 대신 리스트나 튜플 같은 객체의 형태를 쓰는 것도 가능하다. 예를 들어 위 코드의 gen3은 ([0, 0], [1, 1], [2, 4], [3, 9], [4, 16])의 결과를 내는 generator 객체가 된다.


#### 2) for in 구문 뒤에 if 구문 추가된 형태

```python
gen1 = (i for i in range(10) if i%2 == 0)
```

\- for in 구문 뒤에 **if 구문을 추가한 형태**를 쓸 수 있다. 이 경우 각 루프마다 if 뒤의 조건식이 참이 되는지 여부를 확인하고, 참일 때에만 그때에 해당하는 원소를 for in 구문 앞의 연산식에 대입해 이러한 연산식의 결과값으로 구성되는 generator 객체를 생성한다.


#### 3) for in 구문이 중첩된 형태

```python
gen1 = ([i, j] for i in range(2) for j in range(3))
```

\- for in 구문은 중첩해 쓸 수 있으며, 이 경우 앞의 for in 구문의 매 루프마다 뒤의 for in 구문의 루프가 수행되는 식으로 연산이 일어난다. 예를 들어 위 코드의 gen1은 ([0, 0], [0, 1], [0, 1], [1, 0], [1, 1], [1, 2])의 결과를 내는 generator 객체가 된다.



### 3. generator 객체를 통한 코루틴 구현

\- 일반적인 함수 호출 시 호출된 함수 내부의 모든 코드가 수행된 뒤에는 그 함수 내부 코드 수행 과정에서 생성된 모든 변수가 메모리에서 사라지는데, 이는 함수 호출 시 수행된 코드들이 메인 루틴의 서브 루틴으로 간주되기 때문이다. yield 예약어와 generator 객체의 send() 메서드를 활용하여, 함수 내부 모든 코드가 수행된 뒤에도 함수 내부 코드 수행 과정에서 생성된 변수를 메모리에서 지우지 않고 그대로 남기는 형태의 '코루틴'을 새로 만들어 메인 루틴과 별개로 독립된 루틴으로 둘 수 있다. (단, 이렇게 구현하는 코루틴은 동기-블로킹 방식으로 동작한다.)


\- 구체적으로, 다음 방식으로 generator 객체를 통해 코루틴을 구현할 수 있다.

```python
def y_coroutine():
    result = 0
    while True:
        param1, param2 = (yield result)
        result = param1 + param2

yc = y_coroutine() #(1)
yc.__next__() #(2)
print(yc.send([1, 2])) #(3)
```

(1) 코루틴을 구현할 함수를 호출하면 generator 객체가 리턴되는데, 임의의 변수에 이를 저장한다.

(2) 그 generator 객체의 `__next__()` 메서드를 호출하는 코드를 반드시 쓴다. (이때 코루틴 함수 안에서 yield 예약어를 사용한 부분 바로 앞까지 코드와 첫 번째 yield 예약어 호출 코드가 수행된다.)

(3) 코루틴 호출은 send 메서드를 호출한다. 

- send 메서드는 한 개의 인자만 가지므로, 여러 개의 인자를 코루틴에 전달하고자 한다면 리스트 같은 변수형으로 인자를 전달해야 한다.

- send 메서드 호출 시 마지막으로 yield 예약어가 호출된 부분부터 코드가 실행돼 다음 yield 예약어를 사용한 부분 바로 앞까지 코드와 그때의 yield 예약어 호출 코드가 수행된다. 만약 그 다음 yield 예약어를 사용한 코드가 더 나타나지 않는다면 에러가 발생한다. (따라서 generator 객체를 통한 코루틴 구현에서는 대개 코루틴 함수를 while 루프로 감싸게 된다.)