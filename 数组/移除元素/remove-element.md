## 27. 移除元素

#### 1. 题目描述

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

```python
示例 1：

输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
示例 2：

输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

##### 思路：

双指针：**数组、链表、字符串**中常用

1. **快慢指针：使用两个变量作为数组的下标，充当指针作用。**

   **快指针：用于寻找新数组（不包括移除元素）元素**

   **慢指针：指向新数组下标**

2. 左右指针

   左指针：寻找移除元素

   右指针：寻找非移除元素

   最后：将左右指针元素交换即可。（注意交换之后左右指针不动，等到下一个循环时在判断是否移动）

##### 参考答案

1.**快慢指针**

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        index = 0
        for num in nums:
            if num != val:
                nums[index] = num
                index += 1
        return index
```





2. 左右指针

```python
   class Solution:
       def removeElement(self, nums: List[int], val: int) -> int:
           if len(nums) == 0 or nums is None:
               return 0
           left, right = 0, len(nums) - 1
           while left < right:
               while(left < right and nums[left] != val):
                   left += 1
               while left < right  and nums[right] == val:
                   right -= 1
               if nums[left] == val:
                   nums[left], nums[right] = nums[right], nums[left]
           if nums[left] == val:
               return left
           else:
               return left + 1
```