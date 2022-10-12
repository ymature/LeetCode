## 283. 移动零

#### 题目描述

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

```python
示例 1:

输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
示例 2:

输入: nums = [0]
输出: [0]
```

**提示**

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

#####  思路

从结果上看就是要将非零元素，按原来的顺序移动到数组的左边，再将后面赋为零即可。使用**快慢指针**即可（与**移除元素**类似）

##### 参考答案

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if len(nums) == 0 or nums is None:
            return
        index = 0
        for num in nums:
            if num != 0:
                nums[index] = num
                index += 1
        while index < len(nums):
            nums[index] = 0
            index += 1
```

