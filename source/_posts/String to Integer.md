title: String to Integer
id: 24
categories:
  - Programming
date: 2014-12-19 23:56:41
tags:
 - LeetCode简单题
---

很简单的一道题，只是考验细节

<!--more-->

1. 空字符串
2. 去掉串首的空格
3. 如果有+-号，需要忽略或记录
4. 从下一位开始转换数字
5. 转换结果溢出判断

代码写得比较丑。。特殊情况挺多的

```python
class Solution:
    def atoi(self, str):
        if str == '':
            return 0
        i = 0
        while str[i] == ' ':
            i += 1
        str = str[i:]
        i = len(str) - 1
        while str[i] == ' ':
            i -= 1
        str = str[:i+1]
        if not str[0] in ['+', '-', '1', '2', '3', '4', '5', '6', '7', '8', '9', '0']:
            return 0
        i = 0
        for ch in str[1:]:
            i += 1
            if ch not in ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0']:
                break
        str = str[:i+1]
        if str[0] == '-':
            x = -1
            str = str[1:]
        elif str[0] == '+':
            x = 1
            str = str[1:]
        else:
            x = 1
        ans = 0
        for ch in str:
            if not ch in ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0']:
                break
            ans = 10 * ans + int(ch)
        if x * ans > (1 << 31) - 1:
            return (1 << 31) - 1
        elif x * ans < -1 * (1 << 31):
            return -1 * (1 << 31)
        return x * ans
```