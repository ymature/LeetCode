## 257. 二叉树的所有路径

#### 题目描述

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

 **示例1**：

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```python
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

**示例2**：

```python
输入：root = [1]
输出：["1"]
```

**提示**：

- 树中节点的数目在范围 `[1, 100]` 内
- `-100 <= Node.val <= 100`

##### 思路

​	由于需要求得从根节点到叶子结点的路径，所以需要前序遍历，这样才方便父节点到孩子节点的路径的指向。

​	而由于二叉树存在分叉的路径，故需要**回溯**来进行回退--从一个路径进入到另一个路径。

​	前序遍历以及回溯过程·如下：
![257.二叉树的所有路径](https://img-blog.csdnimg.cn/20210204151702443.png)

​	存在递归和迭代两种方法做前序遍历，而回溯在递归过程中很好体现。

###### 1. 递归

​	①、明确函数的参数以及返回值

​		要传入根节点，以及**记录每一个节点的路径path**。这里的递归不需要返回值，不同路径的链接体现在path上。

```python
def getpath(root, path):
```

​	②、明确终止条件

​		当然，如果节点为空，则直接返回。

```python
			if not root:
                return 
```

​	③、明确单层逻辑

​		若是叶子结点，则将path放入到结果集中；如不是叶子结点，将该节点加入到path之后，遍历左右孩子。

```python
			path = path + str(root.val)
    		if not root.left and not root.right:
                paths.append(path)
            else:
                path = path + "->"
                getpath(root.left, path)
                getpath(root.right, path)
```

​		那么，什么时候进行了回溯呢？

回溯：将变量恢复到之前的值。而递归和回溯是一家的，可以选择一边递归一边回溯。

而回溯在该单层逻辑中体现在`path = path + str(root.val)`中。由于path是字符串类型，在python中属于不能修改的类型。

​	因此，将`path`进行修改时会重新创建一个新的对象，即`path = path + str(root.val)`中两个`path`的地址不同（虽然函数的参数`path`传入的是地址）。

​	故，当函数返回时，原调用函数的`path`恢复到原值。

###### 回溯

​	回溯是和递归一家的。回溯的实现可以使用一下两种方法：

- 使用**形参作为参数**（又或者是**创建新对象**）。这样在被调用函数中的修改不会映射到调用函数中，即变量的值会恢复。
- 使用**逆操作**。如`path.push()`，则使用`path.pop()`将进行的操作消除，这样就可以保证返回到调用函数时，变量被恢复。

###### 2. 迭代法

​		除了模拟递归需要一个栈，同时还需要一个栈来存放对应的遍历路径。即一个节点对应一个路径。这样如果节点是叶子结点就可以直接输出路径，否则可以在父节点路径的前提下添加孩子节点。

​		**成对的操作两个栈，才可以保证一个节点对应一个路径。**

​		由于一个节点对应一个路径，同时只需要保证遍历顺序是**从上到下**（以便父节点到孩子节点的路径指向），故可以是**深度遍历中的前序遍历**（根左右，父节点之后才到孩子节点），也可以是**层次遍历**（也是父节点之后才到孩子节点）。

##### 参考答案

1. 递归

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        def getpath(root, path):
            if not root:
                return 
            path = path + str(root.val)
            if not root.left and not root.right:
                paths.append(path)
            else:
                path = path + "->"
                getpath(root.left, path)
                getpath(root.right, path)
        paths = []
        getpath(root, '')
        return paths
```

2. 迭代(前序遍历)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if not root:
            return None
        ret = []
        path = []
        st = []
        st.append(root)
        path.append(str(root.val))
        while st:
            node = st.pop()
            path_node = path.pop()
            if not node.left and not node.right:
                ret.append(path_node)
            if node.right:
                st.append(node.right)
                path.append(path_node + "->" + str(node.right.val))
            if node.left:
                st.append(node.left)
                path.append(path_node + '->' + str(node.left.val))
        return ret
```

