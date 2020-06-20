# Daily Coding Problems

## Day 1
>Given a list of numbers and a number k, return whether any two numbers from the list add up to k.
For example, given [10, 15, 3, 7] and k of 17, return true since 10 + 7 is 17.  
>Bonus: Can you do this in one pass?
```python
from typing import List

k: int = 17
numbers: List[int] = [10, 15, 3, 7]

def check_addup(k: int, numbers: List[int]) -> bool:
    for number in numbers:
        if k - number in numbers:
            return True
    return False

print(check_addup(k, numbers))
```

## Day 2
>Given an array of integers, return a new array such that each element at index i of the new array is the product of all the numbers in the original array except the one at i. For example, if our input was [1, 2, 3, 4, 5], the expected output would be [120, 60, 40, 30, 24]. If our input was [3, 2, 1], the expected output would be [2, 3, 6].  
>Follow-up: what if you can't use division?
```python
import math
from typing import List

numbers1: List[int] = [1, 2, 3, 4, 5]
numbers2: List[int] = [3, 2, 1]

def array_product(numbers: List[int]) -> List[int]:
    prod = math.prod(numbers)
    return [int(prod / i) for i in numbers]

def array_product_nodiv(numbers: List[int]) -> List[int]:
    products = list(numbers)
    for i, _n in enumerate(numbers):
        element = numbers.pop(i)
        products[i] = math.prod(numbers)
        numbers.insert(i, element)
    return products

print(array_product(numbers1))
print(array_product(numbers2))
print(array_product_nodiv(numbers1))
print(array_product_nodiv(numbers2))
```

## Day 3
>Given the root to a binary tree, implement serialize(root), which serializes the tree into a string, and deserialize(s), which deserializes the string back into the tree.  

The Node class and test were given.  
```python
import jsonpickle
# I admit to taking the easy way out on this one ;)

class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def serialize(root: Node) -> str:
    return jsonpickle.encode(root)

def deserialize(s: str) -> Node:
    return jsonpickle.decode(s)

node = Node('root', Node('left', Node('left.left')), Node('right'))
assert deserialize(serialize(node)).left.left.val == 'left.left'
```
