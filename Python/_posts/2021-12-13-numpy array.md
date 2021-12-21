### 1. 생성

```python
import numpy as np
list1 = [1, 2, 3]
tup1 = (4, 5, 6)
arr1 = np.array(list1)
arr2 = np.array(tup1)
```

np.array() 함수에 인자로 리스트, 튜플 등을 넣으면 그 인자와 같은 값을 갖는 numpy의 array형 변수를 선언할 수 있다. numpy의 array형은 빠른 속도를 필요로 하는 연산에 적합하며, numpy의 여러 함수와 연산자를 사용하여 수학의 행렬처럼 사용할 수도 있다.

```python
arr1.ndim  #1이 리턴된다.
arr1.shape #(1, 3)이 리턴된다.
```

ndim 메서드는 array형으로 선언된 그 행렬의 차원을 리턴하고, shape 메서드는 그 행렬의 각 행, 열의 크기를 리턴한다.

```python
arr3 = np.array([[1, 2, 3],[4, 5, 6]])
arr3.reshape(3, 2) #array([[1, 2], [3, 4], [5, 6]])이 리턴된다.
```
reshape 메서드는 array형으로 선언된 그 행렬의 행, 열 크기를 변경한 행렬을 리턴한다.

```python
arr4 = np.zeros((2, 3)) #array([[0, 0, 0], [0, 0, 0]])이 리턴된다.
arr5 = np.ones((2, 3)) #array([[1, 1, 1], [1, 1, 1]])이 리턴된다.
arr6 = np.diag([1, 2, 3]) #array([[1, 0, 0], [0, 2, 0], [0, 0, 3]])이 리턴된다.
arr7 = np.eye(3) #array([[1., 0., 0.], [0., 1., 0.], [0., 0., 1.]])이 리턴된다. 각 성분을 정수형으로 하고 싶다면 인자로 dtype=int를 추가로 넣어야 한다.
```

### 2. array형의 사칙연산

1) 상수와의 연산
```python
arr1 + 1 #array([2, 3, 4])가 리턴된다.
arr1 - 1 #array([0, 1, 2])가 리턴된다.
arr1 * 3 #array([3, 6, 9])가 리턴된다.
arr1 / 3 #array([0.3333, 0.6666, 1])가 리턴된다.
```


2) array형끼리의 연산

```python
arr1 + arr2 #array([5, 7, 9])가 리턴된다.
arr1 - arr2 #array([-3, -3, -3])가 리턴된다.
arr1 * arr2 #array([4, 10, 18])가 리턴된다. 선형대수학에서는 이렇게 성분곱을 하여 행렬을 얻는 연산을 Hadamard product라 한다. 
arr1 / arr2 #array([0.25, 0.4, 0.5])가 리턴된다.
```

array형끼리의 이러한 연산은 크기가 같거나 서로 브로드캐스트가 가능한 크기를 갖는 array형끼리만 가능하다.


3) 행렬곱

```python
np.matmul(arr1, arr2)
arr1 @ arr2
```


matmul 메서드와 @ 연산자는 array형 변수를 인자로 대입할 경우 두 array 사이의 행렬곱 연산을 수행한다.

- dot 메서드도 유사한 기능을 수행하나, 엄밀히 말해 dot 메서드는 **왼쪽 행렬의 행벡터**와 **오른쪽 행렬의 열벡터**의 내적을 수행하여 만든 행렬을 리턴하는 메서드다. 따라서 2차원보다 더 고차원인 array형 변수들, 예를 들어 각 차원의 크기가 (2, 3, 3, **4**)인 array와 (2, 5, **4**, 2)인 array를 서로 dot 연산 시키면 3 x 4 행렬과 4 x 2 행렬의 곱셈을 차례대로 수행한다.

- matmul 메서드 또는 @ 연산자도 왼쪽 array와 오른쪽 array의 각각 가장 오른쪽 끝 두 차원을 행렬의 행과 열로 하는 행렬로 보고 행렬곱 연산을 수행하는 작업은 비슷하지만, 여기서는 이러한 행렬의 개수가 서로 동일해야 한다. 

- numpy의 공식문서는 행렬곱 연산을 수행하려 하는 경우 dot 메서드보다는 matmul 메서드 또는 @ 연산자를 사용할 것을 권한다.


### 3. 브로드캐스트

```python
arr3[1:3, 1] = 0 #arr1이 array([[1, 0], [3, 0], [5, 0]])이 된다. 
```

어떤 array에 대하여 스칼라 또는 차원수가 같거나 작고 다만 각 축의 크기가 그에 해당하는 그 array의 축의 크기와 동일하거나 1인 작은 array를 연산하면 그 스칼라 또는 작은 array의 크기가 확장되어 연산된다. 이를 브로드캐스트라 한다.


### 4. boolean indexing


#### 1) array와 조건식

```python
arr_res = (arr3 > 0) & (arr3 <= 3) 
```

\- array형 변수에 관한 조건식을 쓰면 그 array의 각 성분이 조건식을 만족하는지 여부를 True 또는 False로 판단해 크기가 그 array와 같고 각 행과 열의 성분이 그 결과값을 갖는 새로운 array를 리턴한다. 

\- boolean을 만드는 조건문을 쓸 때에는 비교 연산자 사이를 잇는 논리 연산자로 &, |를 쓴다.

#### 2) boolean indexing

```python
arr_idx = arr3[arr_res] 
```

\- array형 변수의 인덱스에 그 변수와 크기가 같고 각 성분이 True 또는 False인 array를 대입하면, 그 array의 성분 중 그에 대응되는 array의 성분이 True인 성분을 꺼내 그 성분들로 이루어진 1차원 array형 변수를 리턴한다.


### 5. all(), any()

```python
arr_res.all()
arr_res.any()
```

\- array형 변수는 all(), any()를 메서드로서도 갖는다. 메서드로 얻는 결과값은 일반적인 bool 타입이 아니라 numpy의 bool 타입으로 취급된다.


### 6. 전치행렬

```python
t1 = arr3.T #2차원 array에 대해 사용 가능한 attribute로서 그 전치행렬이 리턴된다. 1차원 array의 경우에는 자기 자신이 리턴된다.
t2 = arr1[:, None] #1차원 array를 전치하고자 할 경우, 이러한 방법을 사용하면 이를 전치한 전치행렬이 리턴된다. 2차원 array에 대해서는 이러한 방법을 사용하면 자기 자신이 리턴된다.
```


### * 기타

```python
arr3.trace() #arr3의 대각 성분의 합을 리턴한다.
np.linalg.det(arr6) #arr6의 행렬식 값을 리턴한다.
np.linalg.inv(arr6) #arr6의 역행렬을 리턴한다.
np.linalg.eig(arr6) #arr6의 고유값이 담긴 array와 고유벡터들로 만들어진 행렬을 한 튜플에 담아 리턴한다.
np.linalg.norm(arr1, ord=1) #arr1의 L1 norm을 리턴한다. 인자는 1차원 array뿐 아니라 2차원 array도 가능하다. 이 경우 axis라는 값을 지정하여 파라미터로 전달하면 그 축 방향으로 벡터인 것으로 보고 norm을 계산해 이들을 모은 array를 리턴한다. 한편 ord값을 1이 아니라 2로 지정하면 L2 norm을 리턴한다.
```

