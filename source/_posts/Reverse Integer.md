title: Reverse Integer
id: 22
categories:
  - Programming
date: 2014-12-19 23:54:27
tags:
 - LeetCode简单题
---

很简单的题目

<!--more--> 
第一次提交出错了 因为题目中说如果逆序后的数溢出了，需要返回0
改了又错了 位运算的优先级居然比加减法还要低

```python
class Solution:
    def reverse(self, x):
        s = str(x)
        if x < 0:
            s = s[1:len(s)]
        s = s[::-1]
        if x < 0:
            s = '-' + s
        ans = int(s)
        if ans > (1 << 31) - 1 or ans < - (1 << 31):
            ans = 0
        return ans
```

在Discuss里还看到一个很简单的写法

```python
return (-1 if x < 0 else 1) * int(str(abs(x))[::-1])
```