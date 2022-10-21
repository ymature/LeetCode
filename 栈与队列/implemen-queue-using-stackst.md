## 232. 用栈实现队列

#### 题目描述

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

**说明：**

- 你 **只能** 使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

**示例1**：

```python
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

**提示**：

- `1 <= x <= 9`
- 最多调用 `100` 次 `push`、`pop`、`peek` 和 `empty`
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 `pop` 或者 `peek` 操作）

##### 思路（输入栈+输出栈）

​	这一道题主要考察的是对栈和队列的掌握程度，其中**栈**一般使用list（即线性数组）来实现，只要规定只从一边插入和删除元素即可；**队列**一般使用链表来实现。

​	本题使用栈来模拟队列，如果仅仅使用一个栈肯定是不足够的。因此，需要两个栈，**一个输入栈，一个输出栈**。

​	输入栈：主要担任元素输入作用，在push时将元素输入到输入栈中即可

​	输出栈：主要担任元素输出作用，在pop时将元素从输出栈输出。

​	两者联系：

-  当执行push时，只需将元素输入到输入栈即可

-  当执行pop时，若输出栈为空，将输入栈的所有元素弹出，同时输入到输出栈中，这样就可以将顺序按输入顺序输出；
- 当执行peak时，使用front变量记录输入栈的栈底元素。当输出栈不空时，取栈顶元素即可；当输出栈为空时，输出front变量即可。

过程如下：

​	![image-20221021141730827](https://code-thinking.cdn.bcebos.com/gifs/232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97%E7%89%88%E6%9C%AC2.gif)

##### 参考答案

```python
class MyQueue:
    def __init__(self):
        self.stack_in = []
        self.stack_out = []
        self.front = None

    def push(self, x: int) -> None:
        if not self.stack_in:
            self.front = x
        return self.stack_in.append(x)
        
    def pop(self) -> int:
        if self.stack_out:
            return self.stack_out.pop()
        elif self.stack_in:
            while self.stack_in:
                self.stack_out.append(self.stack_in.pop())
            self.front = None
            return self.stack_out.pop()
        else:
            raise LookupError("Queue is empty")

    def peek(self) -> int:
        if self.stack_out:
            return self.stack_out[-1]
        elif self.stack_in:
            return self.front
        else:
            raise LookupError("Queue is empty")

    def empty(self) -> bool:
        return not (self.stack_in or self.stack_out)

# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

