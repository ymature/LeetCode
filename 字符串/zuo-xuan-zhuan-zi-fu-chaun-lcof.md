## 剑指 Offer 58 - II. 左旋转字符串

#### 题目描述

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

 **示例1**

```python
输入: s = "abcdefg", k = 2
输出: "cdefgab"
```

**示例2**

```python
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```

**限制**

- `1 <= k < s.length <= 10000`

##### 思路

1. 使用切片
2. 不使用切片（**整体反转+局部反转**）

​		步骤如下：

- 反转整个字符串
- 反转区间为后n的子串
- 反转区间为前`len(s) -n` 的子串	

##### 参考答案

1. 

```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        return s[n:] + s[:n]
```

2. 

```python
# 方法三：如果连reversed也不让使用，那么自己手写一个
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        def reverse_sub(lst, left, right):
            while left < right:
                lst[left], lst[right] = lst[right], lst[left]
                left += 1
                right -= 1
        
        res = list(s)
        end = len(res) - 1
        reverse_sub(res, 0, end)
        reverse_sub(res, end -n + 1, end)
        reverse_sub(res, 0, end -n )
        return ''.join(res)
```



