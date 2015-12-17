title: ZigZag Conversion
id: 15
categories:
  - Programming
date: 2014-12-19 23:41:09
tags: 
 - LeetCode简单题
---

看了题解。。直接模拟最简单。。多占一点空间

```python
class Solution:
    def convert(self, s, nRows):
        if nRows == 1:
            return s
        num = 2 * nRows - 2
        answer = ''

        line = s[0::num]
        answer += ''.join(line)

        for i in range(1, nRows - 1):
            line1 = s[i::num]
            line2 = s[num - i::num]
            line = [x , ]
            x = i
            y = num - i
            while x < len(s):
                answer += s[x]
                # There may be an x while y is absent, so we should determine seperately
                if y < len(s):
                    answer += s[y]
                x += num
                y += num

        line = s[nRows - 1::num]
        answer += ''.join(line)
        return answer
```