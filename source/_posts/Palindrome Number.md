title: Palindrome Number
id: 40
categories:
  - Programming
date: 2014-12-26 14:13:28
tags:
 - LeetCode简单题
---

简单题，只是题目要求不能使用额外的空间，也就是说就不能把int转成string这样处理

所有负整数都不算作回文数

用整除和取模把每一位提取出来判断就可以了

<!--more-->

```python
__author__ = 'shijiashuai'
class Solution:
    def isPalindrome(self,x):
        if x < 0:
            return False
        l = 1
        while x / l > 9:
            l *= 10
        r = 1
        while x / l % 10 == x / r % 10:
            if l == r or l == r * 10:
                return True
            l /= 10
            r *= 10
        return False
s = Solution()
print s.isPalindrome(0)
```