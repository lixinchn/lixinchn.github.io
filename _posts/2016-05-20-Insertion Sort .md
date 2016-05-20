---
layout: post
title: Insertion Sort. Python
---

### Algothrim

Give you an unsorted array. Suppose we want an ascending order.

Divide the array into two parts, one is sorted which take left part of the array, another one is unsorted which take right part of the array.

Every time, chose the first element from right part, compare it with elements in left sorted part from end to front one by one, then find the position of that element and insert it into left sorted part.

Insertion sort is stable.


### Animation

![Image of Animation](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

### Python Code

```python
import sys


class InsertionSort():
    def __init__(self):
        self.data = [10, 14, 73, 25, 23, 13, 27, 94, 33, 39, 25, 59, 94, 65, 82, 45]

    def do(self):
        for i in range(len(self.data) - 1):
            target_index = i + 1
            target = self.data[target_index]
            for j in range(i, -1, -1):
                if self.data[j] > target:
                    self.data[j], self.data[target_index] = self.data[target_index], self.data[j]
                    target_index = j
                    continue
                break


if __name__ == '__main__':
    ss = InsertionSort()
    ss.do()
    print ss.data
```
