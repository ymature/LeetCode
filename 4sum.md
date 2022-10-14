## 18. 四数之和

#### 题目描述

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：

- `0 <= a, b, c, d < n`
- `a`、`b`、`c` 和 `d` **互不相同**
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

你可以按 **任意顺序** 返回答案 。

 **示例1**：

```python
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**示例2：**

```python
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```

**提示**：

- `1 <= nums.length <= 200`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`

##### 思路：

使用**排序 + 双指针**方法。

使用**定左右边界**方法：在定左边界`i`的基础的上，再定右边界`j`。注意：由于此题目标`target`可以是任意值，故`target`可以为正数和负数。

- 当`target = -11`， `nums[i] = -5 > target`时，仍可能存在四元组使得`nums[i] + nums[left] + nums[right] + nums[j] == target`（-5， -2， -2， -2）
- 当`target = 10`, `nums[j] = 4 < target`时，仍可能存在四元组使得`nums[i] + nums[left] + nums[right] + nums[j] == target`  (1, 2, 3, 4)

故，不需要在进行剪枝即可。

##### 参考答案

1. **定左右边界**

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        if nums == None or len(nums) < 4:
            return []
        nums.sort()
        ret = []
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            for j in range(len(nums) - 1, i, -1):
                if j < len(nums) - 1 and nums[j] == nums[j + 1]:
                    continue
                left = i + 1
                right = j - 1
                while left < right:
                    if nums[i] + nums[left] + nums[right] + nums[j] > target:
                        right -= 1
                    elif nums[i] + nums[left] + nums[right] + nums[j] < target:
                        left += 1
                    else :
                        ret.append([nums[i], nums[left], nums[right], nums[j]])
                        while left < right and nums[left] == nums[left + 1]:
                            left += 1
                        while left < right and nums[right] == nums[right - 1]:
                            right -= 1
                        left += 1
                        right -= 1
        return ret 
```

2.  **定前两个元素**（此方法可以**拓展**为五数之和、六数之和等等。。。）

```python
# 双指针法
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        res = []
        for i in range(n):
            if i > 0 and nums[i] == nums[i - 1]: 
                continue
            for k in range(i+1, n):
                # 注意此处的去重， k > i + 1：保证了四元组中存在重复元素的情况不会被忽略
                if k > i + 1 and nums[k] == nums[k-1]: 
                    continue
                left = k + 1
                right = n - 1

                while left < right:
                    if nums[i] + nums[k] + nums[left] + nums[right] > target: 
                        right -= 1
                    elif nums[i] + nums[k] + nums[left] + nums[right] < target: 
                        left += 1
                    else:
                        res.append([nums[i], nums[k], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left + 1]: 
                            left += 1
                        while left < right and nums[right] == nums[right - 1]: 
                            right -= 1
                        left += 1
                        right -= 1
        return res
```

