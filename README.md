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

## Day 4
>Given an array of integers, find the first missing positive integer in linear time and constant space. In other words, find the lowest positive integer that does not exist in the array. The array can contain duplicates and negative numbers as well. For example, the input [3, 4, -1, 1] should give 2. The input [1, 2, 0] should give 3. You can modify the input array in-place.  
```python
from typing import List

numbers1: List[int] = [3, 4, -1, 1]
numbers2: List[int] = [1, 2, 0]

def lowest_missing_number(numbers: List[int]) -> int:
    numbers.sort()
    lowest: int = 1
    for i in numbers:
        if i == lowest + 1:
            lowest = i
    return lowest + 1

print(lowest_missing_number(numbers1))
print(lowest_missing_number(numbers2))
```

## Day 5
>cons(a, b) constructs a pair, and car(pair) and cdr(pair) returns the first and last element of that pair. For example, car(cons(3, 4)) returns 3, and cdr(cons(3, 4)) returns 4. Implement car and cdr.  

The implementation of cons was given and therefore not type annotated.
```python
from typing import Callable, Any, TypeVar

def cons(a, b):
    def pair(f):
        return f(a, b)
    return pair

# Big thanks to Florian Bruhin for helping with the type annotations on this one
A = TypeVar('A')
B = TypeVar('B')

def car(pair: Callable[[Callable[[A, B], A]], A]) -> A:
    return pair(lambda a, b: a)

def cdr(pair: Callable[[Callable[[A, B], B]], B]) -> B:
    return pair(lambda a, b: b)

print(car(cons(3, 4)))
print(cdr(cons(3, 4)))
```

## Day 785
>Given a list of integers, return the largest product that can be made by multiplying any three integers.  
>For example, if the list is [-10, -10, 5, 2], we should return 500, since that's -10 * -10 * 5.
```python
from itertools import combinations
from math import prod
from typing import List

numbers: List[int] = [-10, -10, 5, 2]

def largest_list_product(numbers: List[int], n: int = 3):
    """Calculate the largest product of any n integers in a list."""
    if n > len(numbers):
        raise ValueError("Not enough numbers in the list for this product!")
    return max([prod(c) for c in combinations(numbers, n)])

assert largest_list_product(numbers) == 500
assert largest_list_product(numbers, 2) == 100
assert largest_list_product(numbers, 1) == 5
```
