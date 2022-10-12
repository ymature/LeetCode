## 977. 有序数组的平方

#### 题目描述

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

```python
示例 1：

输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
示例 2：

输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

**提示**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非递减顺序** 排序

##### 思路：

1. 从中间开始的双指针，找到第一个大于等于零的数，分别比较两边数的平方的大小
2. 从头尾开始的双指针，比较头尾元素的平方大小，大的即是结果中最大的元素。
3. **元素平方之后进行排序**

##### 参考答案

1. ```python
   class Solution:
       def sortedSquares(self, nums: List[int]) -> List[int]:
           ret = []
           index = len(nums) - 1
           # 可以使用二分查找
           for i, num in enumerate(nums):
               if num >= 0:
                   index = i
                   break
           left, right = index - 1, index
           while left >= 0 and right <= len(nums) - 1:
               if nums[left] ** 2 < nums[right] ** 2:
                   ret.append(nums[left] ** 2)
                   left -= 1
               else:
                   ret.append(nums[right] ** 2)
                   right += 1
           while left >= 0:
               ret.append(nums[left] ** 2)
               left -= 1
           while right <= len(nums) - 1:
               ret.append(nums[right] ** 2)
               right += 1
           return ret
   ```

2. ```python
   class Solution:
       def sortedSquares(self, nums: List[int]) -> List[int]:
           n = len(nums)
           left, right, pos = 0, n - 1, n - 1
           ret = [0] * n
           while left <= right:
               if nums[left] ** 2 > nums[right] ** 2:
                   ret[pos] = nums[left] ** 2
                   left += 1
               else :
                   ret[pos] = nums[right] ** 2
                   right -= 1
               pos -= 1
           return ret
   ```

3. ```python
   class Solution:
       def sortedSquares(self, nums: List[int]) -> List[int]:
           return sorted(num * num for num in nums)
   ```