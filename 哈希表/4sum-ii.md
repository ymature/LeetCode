## 454. 四数相加II

#### 题目描述

给你四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` ，数组长度都是 `n` ，请你计算有多少个元组 `(i, j, k, l)` 能满足：

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

**示例1：**

```python
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**示例2**

```python
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

**提示**

- `n == nums1.length`
- `n == nums2.length`
- `n == nums3.length`
- `n == nums4.length`
- `1 <= n <= 200`
- `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`

##### 思路

**分组 + 哈希表**

形如A + B + ...+ T + ...+ N = 0的式子，转换为(A + ... + T) = -((T + 1) + .. + N)进行计算，其中T为分割点，一般为个数的一半，特殊情况特殊处理。故时间复杂度为O（n ^ (N / 2)）

本题中，将四个数组分为两部分，`nums1`和`nums2`一组，`nums3`和`nums4`一组。

对于`nums1`和`nums2`使用二重循环进行遍历，得到的`nums1[i] + nums2[i]`的值存入哈希映射中作为`key`, 统计对应`nums1[i] + nums2[i]`**出现次数**作为`value`

对于`nums3`和`nums4`，使用二重循环遍历，当遍历到`nums3[i] + nums4[i]`时，查询`-(nums3[i] + nums4[i])`在哈希映射中的`value`,将该`value`（即出现次数）加到`count`中。

最终得到的`count`即为答案

##### 参考答案

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        ret = collections.defaultdict(int)
        count = 0
        for i in nums1:
            for j in nums2:
                ret[i + j] += 1
        for i in nums3:
            for j in nums4:
                key = -i -j 
                count += ret[key]
        return count
```

```python
class Solution:
    def fourSumCount(self, A: List[int], B: List[int], C: List[int], D: List[int]) -> int:
        #简化写法
        countAB = collections.Counter(u + v for u in A for v in B)
        ans = 0
        for u in C:
            for v in D:
                if -u - v in countAB:
                    ans += countAB[-u - v]
        return ans
```

