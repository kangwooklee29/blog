### 1. 파이썬의 비동기 처리


```python
def print_start_to_end(start, end):
    for i in range(start, end):
        print(i)
print_start_to_end(5, 10);
print_start_to_end(20, 30);
```


\- 파이썬의 함수 호출은 기본적으로 동기적-블로킹 방식으로 작동한다. 예를 들어 위 코드의 경우 print_start_to_end(5, 10) 부분에서 함수를 호출했을 때 5부터 9까지 숫자를 다 출력한 그 다음에 비로소 print_start_to_end(20, 30) 부분에서 함수가 호출된다. 

\- 경우에 따라 함수의 호출 및 수행이 비동기적으로 이루어져야 할 때가 있으나, 과거 파이썬에서는 이를 자체적으로 지원하지 않아 외부 모듈을 활용해야 했다. 그러나 파이썬 3.4에서 asyncio가 표준 라이브러리로 추가되고, 파이썬 3.5에서 await/async 예약어가 추가되어 이제 파이썬에서도 자체 문법을 활용한 비동기 프로그래밍을 할 수 있다.

### 2. await/async

```python
async def func1():
    return "나는 사과가 좋아."
```

