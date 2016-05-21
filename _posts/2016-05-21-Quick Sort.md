---
layout: post
title: Quick Sort. Python
---

### Algorithm

Give you an unsorted array.

Each time, the mission is find the position of first element in array. When position is determined, left part is smaller than the element, right part is larger than the element.

Then, repeat the step on left part and right part until array is ordered.

Quick sort is not stable.

### Animation

![Image of Animation](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

### Python Code

```python
import random


class QuickSort():
    def __init__(self):
        # self.data = [10, 14, 73, 25, 23, 13, 27, 94, 33, 39, 25, 59, 65, 82, 45]
        self.data = []
        for i in range(10000):
            self.data.append(int(random.random() * 10000))

    def do(self):
        self.sort(self.data, 0, len(self.data) - 1)

    def sort(self, data, low, high):
        index = self.partition(data, low, high)
        if index >= 0:
            self.sort(data, low, index - 1)
            self.sort(data, index + 1, high)

        
    def partition(self, data, low, high):
        flag = low
        while low <= high:
            while low <= high and data[low] <= data[flag]:
                low += 1
            while low <= high and data[high] >= data[flag]:
                high -= 1
            if low > high:
                data[high], data[flag] = data[flag], data[high]
                return high
            data[low], data[high] = data[high], data[low]
        return -1




if __name__ == '__main__':
    qs = QuickSort()
    qs.do()
    print qs.data

```
