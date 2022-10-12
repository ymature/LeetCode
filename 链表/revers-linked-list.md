## 206. 反转链表

#### 题目描述

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

```python
示例 1：


输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
示例 2：


输入：head = [1,2]
输出：[2,1]
示例 3：

输入：head = []
输出：[]
```

**提示**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

##### 思路

1. 使用**尾插法**

2.  使用**双指针**， 第一个指针`cur`，指向头节点；第二个指针`pre`，初始化为Null

   首先把`cur->next`节点用`tmp`指针保存一下，将`cur->next`指向`pre`，此时已经反转第一个节点了。

   继续移动`cur`和`pre`指针，`pre`指向`cur`，`cur`指向`tmp`，直到`cur`指向null即可。返回`pre`指针。

   过程如下：

   ![image-20221007144935326](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221007144935326.png)

![image-20221007145000629](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221007145000629.png)

##### 参考答案

1. 

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None:
            return head
        tail = head
        while tail.next:
            tail = tail.next
        temp = head
        while temp != tail:
            head = head.next
            temp.next = tail.next
            tail.next = temp
            temp = head
        return head
```

2. 

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur = head   
        pre = None
        while(cur!=None):
            temp = cur.next # 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = pre #反转
            #更新pre、cur指针
            pre = cur
            cur = temp
        return pre
```



