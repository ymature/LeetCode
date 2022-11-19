## 110.平衡二叉树

#### 题目描述

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1 。

**示例1**：

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```python
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**示例2**：

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```python
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

**示例3**：

```python
输入：root = []
输出：true
```

**提示**：

- 树中的节点数在范围 `[0, 5000]` 内
- `-104 <= Node.val <= 104`

##### 思路

###### 题外话

​	这里求的是二叉树的高度而不是深度

强调概念：

- 二叉树节点的深度：指从**根节点到该节点**的最长简单路径边的条数。
- 二叉树节点的高度：指从**该节点到叶子节点**的最长简单路径边的条数。

但leetcode中强调的深度和高度很明显是按照节点来计算的，如图：

![110.平衡二叉树2](https://img-blog.csdnimg.cn/20210203155515650.png)

​	故求深度可以**从上往下**去求，所以需要使用**前序**遍历（根左右），先处理根节点再处理左右子树。

​	而高度只能**从下往上**去求，所以需要使用**后序**遍历（左右根），先求出左右子树的高度才可以求出根节点的高度。

###### 本题思路

 1. **递归**

    ①、明确递归函数的参数和返回值

    ​	参数：子树的根节点

    ​	返回值：子树的高度

    如果左右子树高度差大于1，又该如何表示呢？

    由于返回值为子树的高度，而如果子树不是平衡二叉树，那么返回子树的高度就没有了意义。因此，可以返回`-1`来表示子树不是平衡二叉树。

```python
def getlen(self, root:Optional[TreeNode]) -> int:
```

​		②、明确终止条件

​			遇到空节点为终止，返回0，代表子树的高度为0.

```python
        if not root:
            return 0
```

​		③、明确单层逻辑

​				由于使用后序遍历，需要先递归处理左右子树，之后再处理根节点。由于左右子树可能不是平衡二叉树，此时以根节点为根的二叉树也不是平衡二叉树。故需要直接返回`-1`.

​				如果左右子树都是平衡二叉树就会返回紫薯的子树的高度，根据左右子树的高度差是否大于1来判断返回`-1`还是返回子树高度。

```python
left_len = self.getlen(root.left)
        right_len = self.getlen(root.right)
        if left_len == -1 or right_len == -1:
            return -1
        if abs(left_len - right_len) > 1:
            return - 1
        return max(left_len, right_len) + 1
```

2.  迭代

   不能想[104.二叉树的最大深度](https://programmercarl.com/0104.二叉树的最大深度.html)那样直接使用层次遍历来求得每个节点的深度。

   需要先定义一个函数，专门用来求高度。

   这个函数通过栈模拟的后序遍历找每一个节点的高度（其实是通过求传入节点为根节点的最大深度来求的高度）

   **技巧**：由于模拟后序遍历，首先得到高度的是左右子树，再到根节点。因此，可以使用字典记录每个节点的高度。首先是空节点，初始化高度为0，则叶节点高度为0 + 1 = 1......

##### 参考答案

1. **递归**(重点掌握)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if self.getlen(root) == -1:
            return False
        return True

    def getlen(self, root:Optional[TreeNode]) -> int:
        if not root:
            return 0
        left_len = self.getlen(root.left)
        right_len = self.getlen(root.right)
        if left_len == -1 or right_len == -1:
            return -1
        if abs(left_len - right_len) > 1:
            return - 1
        return max(left_len, right_len) + 1

```

2. 迭代

```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        height_map = {}
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                stack.append(node)
                stack.append(None)
                if node.right: stack.append(node.right)
                if node.left: stack.append(node.left)
            else:
                real_node = stack.pop()
                left, right = height_map.get(real_node.left, 0), height_map.get(real_node.right, 0)
                if abs(left - right) > 1:
                    return False
                height_map[real_node] = 1 + max(left, right)
        return True
```

