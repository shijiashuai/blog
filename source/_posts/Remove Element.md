title: Remove Element
id: 84
categories:
  - Programming
date: 2014-12-29 20:16:22
tags:
 - LeetCode简单题
---

移除list中指定元素，移除后不要求保持顺序，题目的主要要求在于原地修改，即in place
<!--more-->

可以借鉴堆排的想法，把最后一个元素拿过来覆盖掉要移除的元素，然后把list长度减一

```python
__author__ = 'shijiashuai'

class Solution:
    # @param    A       a list of integers
    # @param    elem    an integer, value need to be removed
    # @return an integer
    def removeElement(self, A, elem):
        length = len(A)
        for i in range(length):
            while A[i] == elem and i < length:
                length -= 1
                A[i] = A[length]
        return length
```
