title: Valid Parentheses
id: 66
categories:
  - Programming
date: 2014-12-28 14:22:32
tags:
 - LeetCode简单题
---

经典栈应用

<!--more-->

扫描串，遇到左括号就把它入栈，遇到右括号就从栈中弹出一个括号看是否配对，如果不配对或者此时栈空，返回False

如果扫描完成后栈中还有元素，返回False

扫描完后栈空，返回True

```
__author__ = 'shijiashuai'

class Solution:
    # @return a boolean
    def isValid(self, s):
        stack = []
        match = {
            ')': '(',
            ']': '[',
            '}': '{'
        }
        for ch in s:
            if ch in ['(', '[', '{']:
                stack.append(ch)
            elif not stack or match[ch] != stack.pop():
                return False
        if stack:
            return False
        return True
```