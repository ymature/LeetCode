## 209. 长度最小子数组

#### 题目描述

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 **连续子数组** [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

```python
示例 1：

输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
示例 2：

输入：target = 4, nums = [1,4,4]
输出：1
示例 3：

输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

**提示**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`

#####  思路

**滑动窗口**：**就是不断调整子序列的起始位置和终止位置，从而得到想要的结果**（其实就是使用快慢双指针，快指针：终止位置； 慢指针：起始位置）

只要需要确定如下三点：

- 窗口内是什么？
- 如何移动窗口的起始位置？
- 如何移动窗口的终止位置？

窗口内 就是 满足其和大于等于target的长度最小连续子数组

窗口起始位置移动： 如果窗口中值的和大于等于target时，起始位置向前移一位；

窗口终止位置移动：终止位置就是遍历数组的指针，每次循环都向前移一位。

![209.长度最小的子数组](https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif)

##### 参考答案

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        if nums is None or len(nums) == 0:
            return 0
        lenf = len(nums)+ 1
        start = end = 0
        total = 0
        while end < len(nums):
            # 每次循环加了nums[end]之后，将end++，以便遍历数组
            # 每次循环：窗口内总和小于target，end向前移一位以便增大窗口内总和
            total += nums[end]
            end += 1
            # 窗口内总和大于等于target时，起始位置向前移一位
            # 退出循环时， 窗口内总和小于target
            while total >= target:
                lenf = min(lenf, end - start)
                total -= nums[start]
                start += 1       
        return 0 if lenf == len(nums) + 1 else lenf
```

