## 1. 两数之和

#### 题目描述

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** `target`  的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例1**

```python
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例2**

```python
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例3**

```python
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

##### 思路

由于需要记录数组中的值`value`以及下标`index`；因此，使用**字典**`dict()`,使用`key-value`对来进行记录。该字典用于查找元素的和数是否出现在该字典中，充当了哈希表的作用。

##### 参考答案

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        ret = dict()
        for index, num in enumerate(nums):
            if target - num in ret:
                return [index, ret[target - num]]
            else:
                ret[num] = index
        return []
```



