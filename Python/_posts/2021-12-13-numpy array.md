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


### 3. 행렬곱

```python
np.matmul(arr1, arr2)
arr1 @ arr2
```


matmul 메서드와 @ 연산자는 array형 변수를 인자로 대입할 경우 두 array 사이의 행렬곱 연산을 수행한다. _(한편 dot 메서드도 유사한 기능을 수행하나 numpy의 공식문서는 행렬곱 연산을 수행하려 하는 경우 dot 메서드보다는 matmul 메서드 또는 @ 연산자를 사용할 것을 권한다.)_



### 4. linalg

```python
np.linalg.inv(arr1) #arr1의 역행렬이 리턴된다.
```

