---
title: 'sum, combinations 함수(프로그래머스 > Summer/Winter Coding(~2018) > 소수 만들기)'
---

```python

'''
1. 주어진 리스트에서 3개의 숫자를 골라 더해 만들어진 숫자가 소수일 경우의 수를 구하는 문제.

2. 숫자로 된 iterable 객체를 인자로 받으면 각 원소들의 총합을 리턴하는 내장함수 sum()과 
   반복자를 다루는 모듈인 itertools에서 iterable 객체와 숫자를 인자로 받으면 그 객체의 
   원소들로 주어진 개수만큼 조합을 만들어 튜플형으로 리턴하는 combinations() 함수를 사용하여
   간결하게 코드를 쓸 수 있다.
'''

from itertools import *

def isPrime(num):
    for i in range(2, int(num**0.5)+1):
        if num%i == 0: return False
    return True

def solution(nums):
    answer = 0

    for comb in combinations(nums, 3):
        answer += 1 if isPrime(sum(comb)) else 0

    return answer
```