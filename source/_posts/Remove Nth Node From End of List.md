title: Remove Nth Node From End of List
id: 63
categories:
  - Programming
date: 2014-12-28 14:04:17
tags:
 - LeetCode简单题
---

一道关于链表操作的题目，很有意思。题目要求我们移除链表的倒数第n个元素，遍历一遍就要完成。

<!--more-->

关键的问题就在于，链表有多少个元素，我们在一开始是不知道的，难道必须要扫描到结尾然后倒着往回找第n个元素删除吗？

其实完全可以不用倒回来，我们可以这样想：

假设链表一共有x个元素，倒数第n个元素也就是正数第x-n+1个元素。x的数值我们可以遍历一次得到，既然n是事先知道的，那么x-n+1个元素也一定可以遍历一次得到，方法如下：

1.  设置两个指针，分别为fast和slow，让他们等于链表头head
2.  把fast向后移动n次
3.  这时让fast和slow一起向后移动，直至fast到达链表尾
这样slow就指向了第x-n+1个元素的前一个元素了，也就是第x-n个元素，我们只要让 slow.next = slow.next.next 就可以了

注意如果要移除的是倒数第x个元素，也就是要删除链表头时，只要判断一下返回head.next即可

```python
__author__ = 'shijiashuai'
# # Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # @return a ListNode
    def removeNthFromEnd(self, head, n):
        fast = slow = head
        # jump n times
        for i in range(0, n):
            fast = fast.next
        # if the node to remove is the first one
        if fast is None:
            return head.next
        while fast.next is not None:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return head
```