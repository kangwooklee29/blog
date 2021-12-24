### 1. Series

#### 1) 생성

```python
import pandas as pd

s1 = pd.Series([1, 2, 3, 4])
s2 = pd.Series({'one': 5, 'two':2, 'three':4}, name="random dict")
s1.name = "numbers"
```

\- pd.Series() 함수에 인자로 list 또는 1차원 numpy.ndarray, dictionary형 등을 넣으면 그 인자와 같은 값을 갖는 pandas의 Series형 변수를 선언할 수 있다. pandas의 Series형은 numpy.ndarray, dictionary형과 기능이 매우 유사하지만(예를 들어, pandas의 Series 또한 boolean indexing을 사용할 수 있고 dictionary처럼 get() 메서드를 사용할 수도 있다) 인덱스로 정수만 받는 것이 아니라 dictionary처럼 문자열 등도 인덱스로 받는다는 점이 다르다.

\- pd.Series() 함수에 인자로 dictionary를 넣은 경우, 그 Series형 변수의 각 밸류를 정수 인덱스로 접근할 수도 있다. 예를 들어 위 코드의 s2[0]는 5를 리턴한다.

\- Series형은 name이라는 속성을 가지며, pd.Series()에 인자로 name을 지정해 생성할 때부터 name을 지정할 수도 있다.


#### 2) Series의 메서드들

```python
s1.unique() #s1의 성분 중 유일한 값들을 1차원 numpy.ndarray에 담아 리턴한다.
```


### 2. DataFrame

#### 1) 생성

```python
d1 = pd.DataFrame([1, 2, 3])
d2 = pd.DataFrame({'one':[1], 'two':[2]}, index=['me'])
d3 = pd.read_csv("file_name.csv")
```

\- pd.DataFrame() 함수에 인자로 2차원 이하인 list, numpy.ndarray 또는 **각 키가 갖는 밸류가 그 크기가 모두 동일한 1차원 list 또는 numpy.ndarray인 dictionary**를 넣으면 그 인자로 만든 일종의 DB 테이블을 저장하는 pandas의 DataFrame형 변수를 선언할 수 있다. pandas의 DataFrame형은 다양한 방식으로 데이터를 다룰 수 있는 각종 명령어가 지원되며 또 외부 .csv 파일을 로드해 다룰 수 있는 등 대량의 데이터를 다루기에 매우 적합한 변수형에 해당한다.

\- dictionary로 DataFrame을 만들 경우 키 이름을 열의 이름으로 하는 DB 테이블이 만들어진다. pd.DataFrame() 함수에 인자로 index에 그 DB 테이블의 행의 개수와 길이가 같은 list 등을 전달하면 그 list의 각 성분을 행의 이름으로 하는 DB 테이블이 만들어진다. 행이나 열의 이름을 특별히 지정하지 않는 경우에는 음 아닌 정수가 순서대로 이름으로 매겨진다.

#### 2) 일부만 잘라내기

```python
d3.head(5) #d3의 상위 5개 행의 레코드로 만들어진 DataFrame을 가져온다.
d3.tail(5) #d3의 하위 5개 행의 레코드로 만들어진 DataFrame을 가져온다.
d3["col_name"] #d3의 각 행에서 열 이름이 "col_name"인 값들로 만들어진 Series를 가져온다.
d3[12:25] #d3의 12번 열부터 24번 열까지 Series로 만들어진 DataFrame을 가져온다.
d3[12:25][3:5] #d3의 12번 열부터 24번 열까지 Series로 만들어진 DataFrame의 3, 4번 행을 잘라낸다. numpy.ndarray와 달리 2차원 인덱스를 ,로 구분하는 방식은 사용할 수 없다. 단, iloc을 사용하면 가능하다.
d3[d3["col_name"] == True] #DataFrame 또한 boolean indexing을 사용할 수 있다.
```

#### 3) iloc, loc

```python
d3.iloc[3] #d3의 3번 행의 레코드로 만들어진 Series를 가져온다.
d3.iloc[3, 12] #d3의 3번 행의 12번째 열의 값을 가져온다.
d3.iloc[3:5, 12:25] #d3의 3, 4번 행의 12번 열부터 24번 열까지 잘라낸 DataFrame을 가져온다.
d3.loc[3, "col_name"] #d3의 3번 행의 레코드에서 열 이름이 "col_name"인 값을 가져온다. d3에 행 또는 열에 이름이 따로 지정되지 않았다면 iloc과 똑같이 사용할 수 있으나, 따로 지정된 이름이 있다면 괄호 안에 숫자를 써서는 안된다.
```



### 3. .groupby() 메서드

```python
g1 = d3.groupby(by=d3["col_name"])
```

\- DataFrame 또는 DataFrame에서 열을 특정한 Series에 인자로 by에 특정 열의 Series를 지정하여 .groupby() 메서드를 호출하면 **그 열을 기준으로 그룹지은** 새로운 group 자료형이 만들어진다. 이 자료형을 바로 사용할 수는 없고, **이 뒤에 추가로 그룹함수 메서드를 호출**했을 때 비로소 그 그룹함수의 값으로 이루어진 열을 갖는 새로운 DB 테이블이 생기게 된다. (이해가 안 되면 [클릭](https://lkwks.github.io/db/2021/11/12/SQL-select.html).)

