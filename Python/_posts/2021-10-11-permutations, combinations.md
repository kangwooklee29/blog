```python
from itertools import permutations, combinations
```

#### `permutations()`: **원소들을 배열하는 모든 순서** 찾기 

```python
for perm in permutations(nums, 3):
    print(list(perm))
```

`permutations()` 함수는 주어진 iterable 객체에서 주어진 개수 만큼의 원소를 순서를 고려하여 뽑는 모든 경우의 수를 리턴한다.

이 함수는 예를 들어 주어진 객체 내 **원소들을 배열하는 모든 순서**를 구하는 것 같은 문제를 풀 때 유용하다. 이를 실제로 직접 구현하는 데는 많은 시간이 필요하기 때문이다.



#### `combinations()`: **원소의 조합의 모든 경우의 수** 찾기

```python
for comb in combnations(nums, 3):
    print(list(comb))
```

`combinations()` 함수는 주어진 iterable 객체에서 주어진 개수 만큼의 원소 조합의 모든 경우의 수를 리턴한다.

이 함수는 예를 들어 주어진 객체 내에서 **몇개 원소의 조합의 모든 경우의 수**를 구하는 것 같은 문제를 풀 때 유용하다. 이를 실제로 직접 구현하는 데는 많은 시간이 필요하기 때문이다.

