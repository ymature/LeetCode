## 707. 设计链表

#### 题目描述

设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：`val` 和 `next`。`val` 是当前节点的值，`next` 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 `prev` 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

- get(index)：获取链表中第 `index` 个节点的值。如果索引无效，则返回`-1`。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 `val` 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 `val` 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 `index `个节点之前添加值为 `val`  的节点。如果 `index` 等于链表的长度，则该节点将附加到链表的末尾。如果 `index` 大于链表长度，则不会插入节点。如果`index`小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引` index` 有效，则删除链表中的第 `index` 个节点。

```python
示例：

MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3
linkedList.get(1);            //返回2
linkedList.deleteAtIndex(1);  //现在链表是1-> 3
linkedList.get(1);            //返回3
```

**提示**

- `0 <= index, val <= 1000`
- 请不要使用内置的 `LinkedList` 库。
- `get`, `addAtHead`,` addAtTail`,` addAtIndex` 和 `deleteAtIndex` 的操作次数不超过 `2000`。

##### 思路

使用**虚拟头节点**更方便

单向链表，即每个节点仅存储本身的值和后继节点。除此之外，需要一个哨兵（sentinel）作为虚拟头节点和一个size参数保存链表的有效大小

![image-20221006160933817](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221006160933817.png)

初始化时，只需创建虚拟头节点head和size即可。

实现get(index)时，先判断有效性，再通过循环来找到对应的节点值。过程如下：

![image-20221006161128782](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221006161128782.png)

实现addAtIndex(index, val)时，如果index是有效值，则需要找到下标为index节点的前节点pred， 并创建新节点to_add， 将to_add插入到pre之后。最后需要**更新size大小**。

![image-20221006161409008](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221006161409008.png)

![image-20221006161417355](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221006161417355.png)

实现addAtHead(val)和addAtTail(val)时， 可以借助addAtIndex(index, val)来实现。

实现deleteAtIndex(index)，先判断参数有效性。然后找到下标为index的节点的前驱节点pred, 删除pred后的节点。同时需要**更新size**。过程如下：

![image-20221006161727904](C:\Users\mubai\AppData\Roaming\Typora\typora-user-images\image-20221006161727904.png)

##### 参考答案

```python
# 链表节点结构（使用类来定义）
class ListNode:
    def __init__(self, val, next = None):
        self.val = val
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.size = 0
        self.head = ListNode(0)
    def get(self, index: int) -> int:
        if index >= self.size or index < 0:
            return -1
        cul = self.head
        for _ in range(index + 1):
            cul = cul.next
        return cul.val

    def addAtHead(self, val: int) -> None:
       self.addAtIndex( 0, val)

    def addAtTail(self, val: int) -> None:
        self.addAtIndex(self.size, val)

    def addAtIndex(self, index: int, val: int) -> None:
        if index > self.size:
            return
        index = max(0, index)
        pre = self.head
        for _ in range(index):
            pre = pre.next
        temp = ListNode(val)
        temp.next = pre.next
        pre.next = temp
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return 
        pre = self.head
        for _ in range(index):
            pre = pre.next
        pre.next = pre.next.next
        self.size -= 1
```

