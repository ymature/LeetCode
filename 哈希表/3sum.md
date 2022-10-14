## 15. 三数之和

#### 题目描述

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例1**：

```python
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例2**：

```python
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例3**

```python
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

**提示**：

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

##### 思路

使用**双指针**法，哈希法中去重需要的细节较多，不宜处理。

过程如下：

![image-20221013094115374](https://code-thinking.cdn.bcebos.com/gifs/15.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.gif)

**双指针法的前提**：需要对数组进行排列，若不进行排列，则会导致`nums[i]`可能出现重复情况，导致无法去重。**因此，返回数组中的`值`的题目可以使用，但需要返回`下标`的题目无法使用**

使用**定左边界**的方法，即`i`固定后，其余两个数在`i`的右边进行查找，使用`left`和`right`指针。初始时，在[`left = i + 1`，`right = n - 1`]窗口进行查找。

移动`left`和`right`:

-  如果`nums[i] + nums[left] + nums[right] > 0`，说明三数之和过大，由于`i`在本次窗口中是固定的，而数组是按升序排序好的，因此，`right`下标应该向左移动，让三数之和变小一些
- 如果`nums[i] + nums[left] + nums[right] < 0`, 说明三数之和过小，因此，`left`向右移动，让三数之和变大一些.
- 直到`left`和`right`相遇为止。

**去重**：

说到去重，主要需要考虑一下三个数的去重：`i`、`left`、`right`，对应`nums[i]`、`nums[left]`、`nums[right]`

**`i`的去重**

如果`nums[i]`重复了，由于其为`nums`里遍历的元素,`nums[i]`右边元素相同，则`left`和`right`遍历的元素相同，因此，为避免出现重复三元组，应该直接跳过去。

但有一个问题：是判断 `nums[i]` 与` nums[i + 1]`是否相同，还是判断 `nums[i]` 与 `nums[i-1] `是否相同。不同点：`nums[i]`是比较后一个，还是比较前一个。

若比较后一个：

```python
if nums[i] == nums[i + 1]:
    continue
```

那么就会把三元组中出现重复元素的情况pass掉。例如：{-1, -1, 2},由于会跳过第一个-1, 导致这一个三元组被pass掉。

若比较前一个：

```python
if i > 0 and nums[i] == nums[i - 1]:
    continue
```

{-1, -1, 2}：由于第一个 -1 与前面元素不同，不会跳过第一个-1,这样就可以得到该三元组[-1, -1, 2]

**`left`和`right`去重**

由于三元组不能出现重复的，因此，`left`和`right`不应该遍历重复元素。当找到`nums[i] + nums[left] + nums[right] == 0`时，应该除去重复`left`和`right`之后，再将`left`和`right`同时移动到下一个位置，寻找新的解。

- 对于`nums[i] + nums[left] + nums[right] < 0`，`left`右移是否需要去除重复`nums[left]`,其实并不需要；因为每次`left`右移之后就会重新判断三数之和是否小于零，由于`nums[left]`没变，故三数之和没变还是小于零，导致`left`还会继续右移。

- `right`情况类似

##### 参考答案

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if nums == None or len(nums) < 3:
            return []
        nums.sort()
        ret = []
        for i in range(len(nums)):
            if nums[i] > 0:
                return ret
            # 跳过重复的·i·
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            left = i + 1
            right = len(nums) - 1
            while left < right:
                if nums[i] + nums[left] + nums[right] == 0:
                    ret.append([nums[i], nums[left], nums[right]])
                    # 除去重复解
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    # 寻找新的解
                    left += 1
                    right -= 1
                elif nums[i] + nums[left] + nums[right] > 0:
                    right -= 1
                else :
                    left += 1
        return ret
```

