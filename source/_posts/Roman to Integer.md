title: Roman to Integer
id: 43
categories:
  - Programming
date: 2014-12-26 15:08:57
tags:
 - LeetCode简单题
---

罗马数字转成int，只要知道罗马数字的规则就可以了，没什么难度

<!--more-->

罗马数字规则参见[维基百科](http://zh.wikipedia.org/wiki/%E7%BD%97%E9%A9%AC%E6%95%B0%E5%AD%97)

python里面没有switch语句，我使用了dictionary和匿名lambda函数

```python
__author__ = 'shijiashuai'

class Solution:
    # @return an integer
    def romanToInt(self, s):
        dic = {
            'I': lambda i: -1 if s[i + 1] in ['V', 'X'] else 1,
            'X': lambda i: -10 if s[i + 1] in ['L', 'C'] else 10,
            'C': lambda i: -100 if s[i + 1] in ['D', 'M'] else 100,
            'V': lambda i: 5,
            'L': lambda i: 50,
            'D': lambda i: 500,
            'M': lambda i: 1000,
        }
        x = 0
        s += '@'
        for i, ch in enumerate(s[:-1]):
            x += dic[ch](i)
        return x
s = Solution()
print s.romanToInt('MCMLXXVI')
```