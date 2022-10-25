## 239. 滑动窗口最大值

#### 题目描述

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

**示例1**：

```python
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例2**：

```python
输入：nums = [1], k = 1
输出：[1]
```

**提示**：

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

##### 思路

1. **优先队列**

   对于**最大值**，可以使用一种十分合适的数据结构：**优先队列（堆）**，其中大根堆可以帮助实时维护一系列元素中的最大值。

   本题中，初始时将前k个元素放入优先队列中。每当向右移动窗口时，就把一个新元素放入到队列中，此时堆顶元素就是堆中元素的最大值。然而这个最大值可能并不在滑动窗口中，**这个值在数组中的位置出现在滑动窗口左边界的左侧**。因此，在后续移动中这个值就永远不可能出现在滑动窗口中，可以将其从优先队列删除。

   不断地移除堆顶的元素，直到其确实出现在滑动窗口中。为了方便判断堆顶元素是否在滑动窗口中（还是在之前的窗口中，即当前窗口左边界的左侧），可以在优先队列中存储二元组`（num, index）`表示元素在`num`在数组中的下标`index`，通过`index`可以判断堆顶元素是否在滑动窗口中，还是在滑动窗口之外。

   要点：当元素移出滑动窗口时，不必立刻将其删除。只有当其成为堆顶元素时，才进行比较（通过数组下标）将其删除。

2. 单调队列

   当窗口中有两个下标`i`和`j`，其中`i`在`j`的左侧（i < j）,并且`i`对应的元素不大于`j`对应的元素（nums[i] <= nums[j]）。因此，当窗口右移时，**只要`i`还在窗口中，`j`就一定在窗口中**。同时，**由于nums[j] >= nums[i], nums[i]一定不会是滑动窗口的最大值**，可以将nums[i]永久移除。

​		总结： 当滑动窗口新加入`j`时，在`j`左侧比nums[j]小的元素都可以永久移除，因为nums[j]比其大，窗口中最大值永远不会是在`j`左侧比nums[j]小的元素。

​		实现：

- 			 使用一个队列存储所有没有被移除的下标。在队列中，这些下标按照从小到大排序（即数组左侧元素在队首，右侧元素在队尾）并且它们在数组中对应的值是严格单调递减的（即将左侧小与当前元素的元素移除，剩下元素单调递减）。


-      		 当滑动窗口右移时，需要将一个新的元素放入队列中。为了保持队列的性质，需要不断将新元素和队尾元素进行比较，如果前者大，将队尾元素删除；否则，将其放入队尾。
-      		队首元素即为最大值。但由于队首元素可能会移出滑动窗口，因此当队首元素不在滑动窗口中时，需要将其移除队列。
-      		由于需要在队尾和队首弹出元素，故需要双端队列。

##### 参考答案

1. 优先队列

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        # 注意 Python 默认的优先队列是小根堆
        q = [(-nums[i], i) for i in range(k)]
        # heapq的元素是元组时,第一个参数作为比较因子，输出使用heapq.heappop()
        heapq.heapify(q)

        ans = [-q[0][0]]
        for i in range(k, n):
            heapq.heappush(q, (-nums[i], i))
            while q[0][1] <= i - k:
                heapq.heappop(q)
            ans.append(-q[0][0])
        
        return ans
```

2. 单调队列

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        left = right = 0
        queue = Queue()
        ret = []
        while right < k:
            queue.push(nums[right])
            right += 1
        ret.append(queue.front())
        while right < len(nums):
            queue.pop(nums[left])
            queue.push(nums[right])
            ret.append(queue.front())
            left += 1
            right += 1
        return ret

class Queue:
    def __init__(self):
        self.que = []
    
    def pop(self, value):
        if self.que and self.que[0] == value:
            self.que.pop(0)
    
    def push(self, value):
        while self.que and self.que[-1] < value:
            self.que.pop(-1)
        self.que.append(value)

    def front(self):
        return self.que[0]
```

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        q = collections.deque()
        for i in range(k):
            while q and nums[i] >= nums[q[-1]]:
                q.pop()
            q.append(i)

        ans = [nums[q[0]]]
        for i in range(k, n):
            while q and nums[i] >= nums[q[-1]]:
                q.pop()
            q.append(i)
            while q[0] <= i - k:
                q.popleft()
            ans.append(nums[q[0]])
        
        return ans
```





