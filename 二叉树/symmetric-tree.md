## 101. 对称二叉树

#### 题目描述

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 **示例1**：

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```python
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例2**：

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

```python
输入：root = [1,2,2,null,3,null,3]
输出：false
```

**提示**：

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

##### 思路

​	**首先想清楚，判断对称二叉树要比较的是哪两个节点，要比较的可不是左右节点！**

​	要比较的是根节点的左子树和右子树是不是相互翻转的。**其实比较的是两个树（根的左子树和右子树）**，所以递归遍历的过程中，也是需要同时遍历两颗树。

​	如何比较？比较的是两个子树的里侧和外侧的元素是否相等。

​	![101. 对称二叉树1](https://img-blog.csdnimg.cn/20210203144624414.png)

​	遍历顺序如下：
​	**正是因为要遍历两棵树而且要比较内侧和外侧节点，所以准确的来说是一个树的遍历顺序是根左右，一个树的遍历顺序是根右左**

​	因此，对比的时候需要将相对应位置的节点进行比较

1. **递归法**

   递归三部曲

   ①、 确定递归函数的参数和返回值

   ​		由于比较的是两颗子树，进而判断这个树是否对称。故参数自然就是两棵子树的根节点。

   ​		返回值自然是bool类型。

```python
def compare(self, left:Optional[TreeNode], right:Optional[TreeNode]) -> bool:
```

​	 ②、 确定终止条件

​			**要比较两个节点数值相不相同，首先要把两个节点为空的情况弄清楚！否则后面比较数值的时候就会操作空指针了。**

​			节点为空的情况有：（**注意我们比较的其实不是左孩子和右孩子，所以如下我称之为左节点右节点**）

- 左节点为空，右节点不为空，不对称，return false
- 左不为空，右为空，不对称 return false
- 左右都为空，对称，返回true

​			左右节点不为空：

- 左右都不为空，比较节点数值，不相同就return false
- 左右都不为空，比较节点数值，相同则展开左右节点的对应左右孩子，以便继续比较

```python
        if not left and not right:
            return True
        elif not left and right:
            return False
        elif left and not right:
            return False
        elif left.val != right.val: #  在后面处理左右节点数值相同的情况
            return False
```

​		③、确定单层递归的逻辑

​				单层递归逻辑就是处理 左右节点都不为空且数值相同的情况	

- 比较二叉树外侧是否对称：传入的是左节点的左孩子，右节点的右孩子。
- 比较内测是否对称，传入左节点的右孩子，右节点的左孩子。
- 如果左右都对称就返回true ，有一侧不对称就返回false 。

```python
		outside = self.compare(left.left, right.right)
        inside = self.compare(left.right, right.left)
        return outside & inside # 需内侧和外侧均对称，该节点才对称
```

2. **迭代法**

   这里并不能使用前中后序的迭代法。但可以使用队列或栈来比较两棵树（根节点的左右子树）是否相互翻转

   **这个迭代法，其实是把左右两个子树要比较的元素顺序放进一个容器，然后成对成对的取出来进行比较**

###### 	使用队列

​		通过队列来判断根节点的左子树和右子树的内侧和外侧是否相等。将相应位置的节点相邻放入队列中，之后两两对比即可。

​		动画如下：

![101.对称二叉树](https://tva1.sinaimg.cn/large/008eGmZEly1gnwcimlj8lg30hm0bqnpd.gif)

######   使用栈

​		使用栈与使用队列的逻辑一致，只是容器不同罢了。

##### 参考答案

1. 递归法

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        return self.compare(root.left, root.right)
        
    def compare(self, left:Optional[TreeNode], right:Optional[TreeNode]) -> bool:
        if not left and not right:
            return True
        elif not left and right:
            return False
        elif left and not right:
            return False
        elif left.val != right.val:
            return False
        
        outside = self.compare(left.left, right.right)
        inside = self.compare(left.right, right.left)
        return outside & inside
```

2. 队列

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        st = deque()
        st.append(root.left)
        st.append(root.right)
        while st:
            node1 = st.popleft()
            node2 = st.popleft()

            if not node1 and not node2:
                continue
            elif not node1 and node2:
                return False
            elif node1 and not node2:
                return False
            elif node1.val != node2.val:
                return False
            st.append(node1.left)
            st.append(node2.right)
            st.append(node1.right)
            st.append(node2.left)
        return True
```

3. 栈

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        st = []
        st.append(root.left)
        st.append(root.right)
        while st:
            node1 = st.pop()
            node2 = st.pop()

            if not node1 and not node2:
                continue
            elif not node1 and node2:
                return False
            elif node1 and not node2:
                return False
            elif node1.val != node2.val:
                return False
            st.append(node1.left)
            st.append(node2.right)
            st.append(node1.right)
            st.append(node2.left)
        return True
```

