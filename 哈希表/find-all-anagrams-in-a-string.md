## 438. 找到字符串中所有字母异位词

#### 题目描述

给定两个字符串` s` 和 `p`，找到 `s` 中所有 `p` 的**异位词 **的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**异位词** 指由相同字母重排列形成的字符串（包括相同的字符串）。

**示例1：**

```python
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

**示例2**：

```python
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

**提示**：

- `1 <= s.length, p.length <= 3 * 104`
- `s` 和 `p` 仅包含小写字母

##### 思路

1. 滑动窗口

   由于需要在`s`中寻找字符串`p`的异位词，而`p`的异位词的长度与`p`长度一致，所以需要在`s`中构造一个长度与`p`相同的滑动窗口，**并维持滑动窗口中的每种字母的数量；**

   当滑动窗口中每种字母的数量与字符串`p`中·每种字母的数量相同时，说明滑动窗口为可行窗口，窗口内为字符串`p`的异位词

   实现：需要一个数组就记录`p`中每种字母数量；需要另一个数组记录滑动窗口中每种字母的数量。

   移动：向前移一位时，右边界有新字母进入，左边界有字母移除

2. 优化滑动窗口

   不再分别统计滑动窗口和`p`中每种字母的数量，而是统计滑动窗口和`p`每种字母的数量差；并引入变量`differ`来记录当前窗口与`p`中数量不同的**字母个数**，并维护

   实现：需要一个数组记录每种字母的需求数量，`p`中字母使数组数量减一，`s`中字母使数组数量加一；

    因此，数组中`count[i]`大于零：滑动窗口中该字母数量比`p`中该字母数量多；`count[i]`小于零：`p`中该字母数量比滑动窗口中该字母数量多；`count[i]`等于零：滑动窗口中该字母数量与`p`中该字母数量一致。

   `differ`：`count`数组中**不等于**零的字母个数。

##### 参考答案

1.

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        if len(s) < len(p):
            return []
        ret = []
        countp  = [0] * 26
        counts = [0] * 26
        for i in range(len(p)):
            countp[ord(p[i]) - ord('a')] += 1
            counts[ord(s[i]) - ord('a')] += 1
        if counts == countp:
            ret.append(0)
        for i in range(len(s) - len(p)):
            counts[ord(s[i]) - ord('a')] -= 1
            counts[ord(s[i + len(p)]) - ord('a')] += 1
            if counts == countp:
                ret.append(i + 1)
        return ret
```

2. 

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        s_len, p_len = len(s), len(p)

        if s_len < p_len:
            return []

        ans = []
        count = [0] * 26
        for i in range(p_len):
            count[ord(s[i]) - 97] += 1
            count[ord(p[i]) - 97] -= 1

        differ = [c != 0 for c in count].count(True)

        if differ == 0:
            ans.append(0)

        for i in range(s_len - p_len):
            if count[ord(s[i]) - ord('a')] == 1:  # 窗口中字母 s[i] 的数量与字符串 p 中的数量从不同变得相同
                differ -= 1
            elif count[ord(s[i]) - 97] == 0:  # 窗口中字母 s[i] 的数量与字符串 p 中的数量从相同变得不同
                differ += 1
            count[ord(s[i]) - 97] -= 1

            if count[ord(s[i + p_len]) - 97] == -1:  # 窗口中字母 s[i+p_len] 的数量与字符串 p 中的数量从不同变得相同
                differ -= 1
            elif count[ord(s[i + p_len]) - 97] == 0:  # 窗口中字母 s[i+p_len] 的数量与字符串 p 中的数量从相同变得不同
                differ += 1
            count[ord(s[i + p_len]) - 97] += 1
            
            if differ == 0:
                ans.append(i + 1)

        return ans
```

