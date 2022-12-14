## 02.07 链表相交

#### 题目描述

给你两个单链表的头节点` headA `和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

图示两个链表在节点 c1 开始相交：

![image-20221005192003407](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)



题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

```python
示例1：
```

![image-20221005192003407](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```python
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
示例2：
```

![image-20221005192003407](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```python
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
示例3：
```

![image-20221005192003407](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```python
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

**提示**：

- `listA` 中节点数目为 `m`
- `listB` 中节点数目为 `n`
- `0 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- 如果 `listA` 和 `listB` 没有交点，返回NULL
- 如果 `listA` 和 `listB` 有交点，`intersectVal == listA[skipA + 1] == listB[skipB + 1]`

##### 思路

1. 对齐**链表末尾**

   由于链表的公共部分位于链表的**末尾**。因此，将链表的末尾对齐，以长度较短的链表位置开始。如下所示

   ![image-20221008162504253](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221008162504253.png)

​	使同时出发时走过不相交的结点个数一致，`curA`=`curB`时即是答案。

2. **双指针**遍历两链表

   设「第一个公共节点」为` node` ，「链表 `headA`」的节点数量为 a ，「链表 `headB`」的节点数量为 b ，「两链表的公共尾部」的节点数量为 c ，则有：

   - 头节点 `headA` 到 `node` 前，共有 `a - c` 个节点；

   - 头节点 `headB` 到 `node` 前，共有 `b - c`个节点；

   ![image-20221008162504253](https://pic.leetcode-cn.com/1615224578-EBRtwv-Picture1.png)

考虑构建两个节点指针`A`,`B`分别指向两链表头节点`headA`,`headB`,做如下操作：

- 指针`A`先遍历完链表`headA`,再开始遍历链表`headB`,当走到`node`时，经过的步数为：a + (b - c)
- 指针`B`先遍历完链表`headB`,再开始遍历链表`headA`,当走到`node`时，经过的步数为：b + (a - c)

如下式所示，此时指针 `A` ,` B` 重合，并有两种情况：

​							a + (b - c) = b + (a - c)

- 若两链表 **有 **公共尾部 (即 c > 0 ) ：指针 `A` ,`B` 同时指向「第一个公共节点」`node` 。
- 若两链表 **无** 公共尾部 (即 c = 0 ) ：指针 `A` , `B` 同时指向 null 。

##### 参考答案

1.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if headA == None or headB == None:
            return None
        p = headA
        q = headB
        sizeA = sizeB = 0
        while p:
            p = p.next
            sizeA += 1
        while q:
            q = q.next
            sizeB += 1
        p = headA
        q = headB
        if sizeA > sizeB:
            for _ in range(sizeA - sizeB):
                p = p.next
        else:
            for _ in range(sizeB - sizeA):
                q = q.next
        while p != q:
            p = p.next
            q = q.next
        return p
```

2.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if headA == None or headB == None:
            return None
        A = headA
        B = headB
        while A != B:
            A = A.next if A else headB
            B = B.next if B else headA
        return A
```

