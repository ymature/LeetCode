## 704. 二分查找

#### 1. 题目描述

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

```python
示例 1:

输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
示例 2:

输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

**提示：**

1. 你可以假设 `nums` 中的所有元素是不重复的。
2. `n` 将在 `[1, 10000]`之间。
3. `nums` 的每个元素都将在 `[-9999, 9999]`之间。

##### 思路：两种二分查找区间方法

1.  **区间：左闭右闭（[left, right]）**
   - **while(left <= right) 要使用 <=，因为left==right时，区间有意义。**
   - **if (nums[middle] > target)，right 赋予middle - 1，因为nums[middle]一定不是target，而区间为[left, right]，故接下来在区间[left, middle-1]中查找。**

 	2. 区间： 左闭右开（[left, right)）
     - while (left < right)使用< ， 因为left == right时，区间[left, right)无意义
     - if(nums[middle] > target) right赋予middle，因为nums[middle]不等于target，而区间又是[left, right)，故将区间[left, middle)只取left ~ middle-1的值。

​	相同： if（nums[middle] < target）, left赋予middle + 1，因为区间左边都是闭区间。

##### 参考答案

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while(left <= right) :
            middle = (left + right) // 2
            if (nums[middle] < target) :
                left = middle + 1
            elif nums[middle] > target:
                right = middle - 1
            else:
                return middle
        return -1
```



