title: Remove Duplicates from Sorted Array
id: 68
categories:
  - Programming
date: 2014-12-28 14:38:52
tags:
 - LeetCode简单题
---

一开始没看懂题目，原来要修改传入的A数组然后返回修改后的A数组长度

<!--more-->

```python
__author__ = 'shijiashuai'
class Solution:
    # @param a list of integers
    # @return an integer
    def removeDuplicates(self, A):
        num = None
        count = 0
        for n in A:
            if n != num:
                A[count] = n
                count += 1
                num = n
        return count
```