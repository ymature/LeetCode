## 34. [在排序数组中查找元素的第一个和最后一个位置]

#### 题目描述

给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。

```python
示例 1：

输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
示例 2：

输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
示例 3：

输入：nums = [], target = 0
输出：[-1,-1]
```

**提示**：

- 0 <= nums.length <= 10^5

- -10^9 <= nums[i] <= 10^9
- nums 是一个**非递减数组**
- -10^9 <= target <= 10^9

##### 思路

使用二分查找，先查找到nums数组中是否存在target，若存在，再使用双指针左右滑动寻找最大区间。

##### 参考答案

```python
# 首先使用二分查找，查找target位置
# 如果二分查找失败，则 binarySearch 返回 -1，表明 nums 中没有 target。此时，searchRange 直接返回 {-1, -1}；
# 如果二分查找成功，则 binarySearch 返回 nums 中值为 target 的一个下标。然后，通过**左右滑动指针**，来找到符合题意的区间
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if nums is None or len(nums) == 0:
            return [-1, -1]

        def binarySearch(nums:List[int], target:int) -> int:
            left, right = 0, len(nums) - 1
            while left <= right:
                middle = (left + right) // 2
                if nums[middle] < target:
                    left = middle + 1
                elif nums[middle] > target:
                    right = middle - 1
                else:
                    return middle
            return -1
        ret = binarySearch(nums, target)            
        if ret == -1:
            return [-1, -1]
        else:
            start, end  = ret, ret
            # 先查看是否到了数组边界，以及下一个是否为target
            while start - 1 >= 0 and nums[start - 1] == target:
                start -= 1
            while end + 1 <= len(nums) - 1 and nums[end + 1] == target:
                end += 1
            return [start, end]

```

