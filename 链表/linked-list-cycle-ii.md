## 142. 环形链表II

#### 题目描述

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 `pos` 是` -1`，则在该链表中没有环。**注意**：`pos` **不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



**示例1：**

![image-20221005192003407](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```python
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例2**：

![image-20221005192003407](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```python
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例3**：

![image-20221005192003407](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```python
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环
```

**提示**：

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

##### 思路

分两步：

1.  判断链表是否有环

   使用**快慢**指针，`fast`指针每次移动两个节点，`slow`指针每次移动一个节点。

   若链表中有环，一定是`fast`指针先进入环中，之后`slow`指针进入环后，一定在环中相遇。

   因此，若`fast`指针和`slow`指针相遇，则说明一定存在环。若`fast->next`或`fast->next->next`为NULL，说明存在链表结尾，不存在环

   过程如下：

   ![image-20221005192003407](https://tva1.sinaimg.cn/large/008eGmZEly1goo4xglk9yg30fs0b6u0x.gif)

2. 判断环的入口（**数学分析**）

   假设从头结点到环形入口节点 的节点数为`x`。 环形入口节点到 `fast`指针与`slow`指针相遇节点 节点数为`y`。 从相遇节点 再到环形入口节点节点数为` z`。 如图所示：

   ![image-20221005192003407](https://img-blog.csdnimg.cn/20210318162938397.png)

   那么，相遇时：`slow`指针走过的节点数：`x + y`;`fast`指针走过的节点数：`x + y + n * (y + z)`, n为`fast`指针走过的环的圈数。

   又因为`fast`指针每次移动两个节点，`slow`指针每次移动一个节点，故：`(x + y) * 2 = x + y + n * (y + z) `。

   两边化简： `x + y = n * (y + z)`

   因为寻找环的入口即`x`,因此`x = n *(y + z) - y = (n - 1) *(y + z) + z`.由于`fast`指针至少在环中走过了一圈，故 n 大于等于1.

   这就意味着： **从头结点出发一个指针`index1`，从相遇节点也出发一个指针`index2`，每次都只移动一个节点；当`index1`从头结点移动到环入口时，`index2`也走了`n - 1`圈加上`z`步；此时，`index1`和`index2`相遇位置就是环的入口**

   过程如下：
   ![image-20221005192003407](https://tva1.sinaimg.cn/large/008eGmZEly1goo58gauidg30fw0bi4qr.gif)

##### 参考答案

1.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None:
            return None
        fast = slow = head
        while fast.next and fast.next.next:
            fast = fast.next.next
            slow = slow.next
            # 相遇->有环
            if fast == slow:
                index1 = fast
                index2 = head
                # 环的入口
                while index1 != index2:
                    index1 = index1.next
                    index2 = index2.next
                return index1
        return None
```

