title: Longest Common Prefix
id: 46
categories:
  - Programming
date: 2014-12-26 15:44:52
tags:
 - LeetCode简单题
---

第一次提交，没想到strs是空的情况（我直接取了strs的第一个元素）
第二次提交，没想到strs的第一个串可能不是最短的。

<!--more-->

```python
__author__ = 'shijiashuai'
class Solution:
    # @return a string
    def longestCommonPrefix(self, strs):
        if strs == []:
            return ''
        prefix = ''
        for i, ch in enumerate(strs[0]):
            for s in strs[1:]:
                if len(s) < i+1:
                    return prefix
                if s[i] != ch:
                    return prefix
            prefix += ch
        return prefix
s = Solution()
print s.longestCommonPrefix(['a','abc','ab'])
```