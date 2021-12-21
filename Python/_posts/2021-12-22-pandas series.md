### 1. 생성

```python
import pandas as pd

s1 = pd.Series([1, 2, 3, 4])
s2 = pd.Series({'one': 5, 'two':2, 'three':4}, name="random dict")
s1.name = "numbers"
```

\- pd.Series() 함수에 인자로 list 또는 1차원 numpy.ndarray, dictionary형 등을 넣으면 그 인자와 같은 값을 갖는 pandas의 Series형 변수를 선언할 수 있다. pandas의 Series형은 numpy.ndarray, dictionary형과 기능이 매우 유사하지만(예를 들어, pandas의 Series 또한 boolean indexing을 사용할 수 있고 dictionary처럼 get() 메서드를 사용할 수도 있다) 인덱스로 정수만 받는 것이 아니라 dictionary처럼 문자열 등도 인덱스로 받는다는 점이 다르다.

\- pd.Series() 함수에 인자로 dictionary를 넣은 경우, 그 Series형 변수의 각 밸류를 정수 인덱스로 접근할 수도 있다. 예를 들어 위 코드의 s2[0]는 5를 리턴한다.

\- Series형은 name이라는 속성을 가지며, pd.Series()에 인자로 name을 지정해 생성할 때부터 name을 지정할 수도 있다.