## 35. 搜索插入位置

#### 题目描述：

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

```python
示例 1:

输入: nums = [1,3,5,6], target = 5
输出: 2
示例 2:

输入: nums = [1,3,5,6], target = 2
输出: 1
示例 3:

输入: nums = [1,3,5,6], target = 7
输出: 4
```

**提示**：

- 1 <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- nums 为 无重复元素 的 升序 排列数组
- -10^4 <= target <= 10^4

##### 思路：

由于要求使用时间复杂度为O(log n)的算法。因此可以使用二分查找算法解决。

##### 参考答案

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            middle = (left + right) // 2
            if nums[middle] < target:
                left = middle + 1
            elif nums[middle] > target:
                right = middle - 1
            else:
                return middle
        return left
```

