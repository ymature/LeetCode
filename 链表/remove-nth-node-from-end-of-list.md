## 19. 删除链表的倒数第N个节点

#### 题目描述

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

```python
示例1：
```

![image-20221008102222478](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```python
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
示例 2：

输入：head = [1], n = 1
输出：[]
示例 3：

输入：head = [1,2], n = 1
输出：[1]
```

**提示**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

##### 思路

**虚拟头节点**：方便**删除实际头节点**的逻辑

使用**快慢指针**：

- 初始时，`fast`、`slow`均指向虚拟头节点。
- `fast`先走`n`步，之后`fast`、`slow`同时向前移动；
- 当`fast`是最后一个节点（即`fast->next`=NULL）时，`slow`指向要删除节点的前一个节点

##### 参考答案

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        if head == None:
            return 
        ret = ListNode(0, head)
        fast = slow = ret
        # fast先走n步
        while n and fast:
            fast = fast.next
            n -= 1
        if n != 0:
            return head
        # fast指向最后一个节点，slow指向要删除节点的前一个节点
        while fast.next:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return ret.next
```

