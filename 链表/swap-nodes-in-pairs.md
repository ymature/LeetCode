## 24. 两两交换链表中的节点

#### 题目描述

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

```python
示例1：
```

![image-20221007153610004](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221007153610004.png)

```python
输入：head = [1,2,3,4]
输出：[2,1,4,3]
示例 2：

输入：head = []
输出：[]
示例 3：

输入：head = [1]
输出：[1]
```

**提示**：

- 链表中节点的数目在范围 `[0, 100]` 内
- `0 <= Node.val <= 100`

##### 思路：

1. **迭代**

   使用**虚拟头节点**，要不然每次针对头节点（没有前一个指针指向头节点），还要单独处理

   接下来就是交换相邻两个元素，**此时一定要画图，不画图，操作多个指针很容易乱，而且要操作的先后顺序**。

   初始时，`cur`指向**虚拟头节点**，然后进行如下三步：（其中步骤二和三应该执行顺序相反，即**先执行步骤三，再执行步骤二**）

   ![image-20221007154127886](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221007154127886.png)

   操作之后：

   ![image-20221007154157838](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221007154157838.png)

  2. 递归

     递归写法要观察本级递归的解决过程，形成抽象模型，因为递归本质就是不断重复相同的事情。而不是去思考完整的调用栈，一级又一级，无从下手。我们应该关注**一级调用小单元**的情况，也就是单个f(x)。

     其中我们应该关心的主要有三点：

     1. 返回值
     2. 调用单元做了什么
     3. 终止条件

      在本题中：

     1. 返回值：交换完成的子链表

     2. 调用单元：设需要交换的两个点为 head 和 next，head 连接后面交换完成的子链表，next 连接 head，完成交换
     3. 终止条件：head 为空指针或者 next 为空指针，也就是当前无节点或者只有一个节点，无法进行交换

     

##### 参考答案

1.

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        res = ListNode(next=head)
        cur = res
        
        # 必须有pre的下一个和下下个才能交换，否则说明已经交换结束了
        while cur.next and cur.next.next:
            pre = cur.next
            post = cur.next.next
            
            # cur，pre，post对应最左，中间的，最右边的节点
            cur.next = post
            pre.next = post.next
            post.next =  pre

            cur = cur.next.next
        return res.next
```

2.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        if head == None or head.next == None:  
            return head
        rest = head.next.next
        newHead = head.next
        newHead.next = head
        head.next = self.swapPairs(rest)
        return newHead        
```



