```python
from itertools import permutations, combinations


nums = [1, 2, 3, 4, 5, 6]
for perm in permutations(nums, 3):
    print(list(perm))

for comb in combnations(nums, 3):
    print(list(comb))
```