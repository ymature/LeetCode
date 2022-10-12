## 203. 移除链表元素

#### 题目描述

给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

 示例 1：

![image-20221005192003407](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221005192003407.png)

```python
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
示例 2：

输入：head = [], val = 1
输出：[]
示例 3：

输入：head = [7,7,7,7], val = 7
输出：[]
```

**提示**

- 列表中的节点数目在范围 `[0, 104]` 内
- `1 <= Node.val <= 50`
- `0 <= val <= 50`

##### 思路（注意**头节点**的删除即可）

删除节点分为两类情况：

- 删除**头节点**

- 删除其余节点

删除头节点有以下方法：

1. 直接使用原来的链表来进行删除操作（单独处理头节点）

​	只需要将头节点向后移动一位就可以了，这样就从链表中移除了一个头结点，过程如下：

![image-20221005221116456](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221005221116456.png)

![image-20221005221131933](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221005221131933.png)

2. 设置一个**虚拟头节点**进行删除操作（可以将删除其余节点和删除头节点归为同一种操作）

​	首先在头节点之前创建一个虚拟头节点作为新的头节点，因此删除头节点与删除其余节点操作一致。

​	最后，return头结点时，返回`return dummyNode->next` ,这才是真正的新头节点。

​	过程如下：
![image-20221005221604431](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221005221604431.png)

（3）第三种方法：递归（难理解）

##### 参考答案

1。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        while head and head.val == val:
            head =  head.next
        if head == None:
            return head
        pre = head
        while pre.next:
            if pre.next.val == val:
                pre.next = pre.next.next
            else:
                pre = pre.next
        return head        
```

2. 

```python
class Solution2:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        pre_node = ListNode(next=head)
        node = pre_node
        while node.next:
            if node.next.val == val:
                node.next = node.next.next
            else:
                node = node.next
        return pre_node.next
```

3. 

```python
class Solution3:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        if head is None:
            return head
        head.next = self.removeElements(head.next, val)
        # 利用递归快速到达链表尾端，然后从后往前判断并删除重复元素
        return head.next if head.val == val else head
        # 每次递归返回的为当前递归层的head(若其值不为val)或head.next
        # head.next及之后的链表在深层递归中已经做了删除值为val节点的处理，
        # 因此只需要判断当前递归层的head值是否为val，从而决定head是删是留即可
```

