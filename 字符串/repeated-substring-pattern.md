## 459. 重复的子字符串

#### 题目描述

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

**示例1**

```python
输入: s = "abab"
输出: true
解释: 可由子串 "ab" 重复两次构成。
```

**示例2**

```python
输入: s = "aba"
输出: false
```

**示例3**

```python
输入: s = "abcabcabcabc"
输出: true
解释: 可由子串 "abc" 重复四次构成。 (或子串 "abcabc" 重复两次构成。)
```

**提示**：

- `1 <= s.length <= 104`
- `s` 由小写英文字母组成

##### 思路

1. 暴力枚举

   如果长度为n的字符串`s`可以由长度为`n'`的子串`s'`重复多次构成，则：

   - n一定是`n'`的倍数
   - `s'`一定是s的前缀
   - 对于任意*i*∈[*n*′,*n*)，有s[i] = s[i - n']

​	也就是说，s中长度为n'的前缀一定是s'，并且之后的每一个位置上的s[i]，都与他之前的第n'个字符s[i - n']相同。

​	因此，可以从小到大枚举n', 并对s进行遍历。同时由于子串至少重复一次，故子串长度n'小于n的一半。

2. 移动匹配 (**s+s**）

   当一个字符串s: abcabc，内部由重复的子串组成时，那么这个字符串的结构一定是由一个个`s'`构成，因此，前面的子串和后面的子串相同。当两个s连在一起时，中间的字串中必定可以再找到一个字符串s。如下图所示

   ![image1](https://code-thinking-1253855093.file.myqcloud.com/pics/20220728104518.png)

![image](https://code-thinking-1253855093.file.myqcloud.com/pics/20220728104931.png)

​		所以判断字符串s是否由重复的子串组成，只需要将两个s拼接在一起，里面还出现一个字符串s的话，就说明s是由重复子串组成。

​		当然，要除去**s+s的首字符和尾字符**，这样才可以避免搜索出原来的s。

3. KMP

   在**s+s**字符串中间查找是否还存在字符串s，这样的查找就是字符串匹配问题，可以使用KMP进行解决。

4. KMP的优化

   首先理解什么是前缀和后缀：

   前缀：**不包含最后一个字符**的所有以第一个字符开头的连续子串；

   后缀：**不包含第一个字符**的所有以最后一个字符结尾的连续子串；

   在可以由重复子串`s'`组成的字符串`s`中，假设该`s`可以由n个`s'`组成，则最长公共前后缀分别包含`n - 1`个`s'`，如图所示：

   ![image](https://code-thinking-1253855093.file.myqcloud.com/pics/20220728205249.png)

​		因此，该字符串`s`除去最长相等前缀或后缀之后剩下的子串就是最小重复子串。

​		又由于需要存在最长相等前后缀，因此需要确保next[len - 1] != -1,这样才可以确保存在最长相等前缀。

​		由于s可以由n个`s'`组成，而最小单元`s'`的长度为：len(s) - (next[len(s) - 1] + 1)。所以s的长度应为`s'`长度的倍数：`len(s) % [len(s) - (next[len(s) - 1] + 1)] = 0`

 		那s的长度为`s'`长度的倍数时就一定是可以由`s'`组成吗？

​		![image](https://code-thinking-1253855093.file.myqcloud.com/pics/20220728212157.png)

​			步骤一：因为 这是相等的前缀和后缀，t[0] 与 k[0]相同， t[1] 与 k[1]相同，所以 s[0] 一定和 s[2]相同，s[1] 一定和 s[3]相同，即：，s[0]s[1]与s[2]s[3]相同 。

​			步骤二： 因为在同一个字符串位置，所以 t[2] 与 k[0]相同，t[3] 与 k[1]相同。

​			步骤三： 因为 这是相等的前缀和后缀，t[2] 与 k[2]相同 ，t[3]与k[3] 相同，所以，s[2]一定和s[4]相同，s[3]一定和s[5]相同，即：s[2]s[3] 与 s[4]s[5]相同。

​			步骤四：循环往复。

​			所以字符串s，s[0]s[1]与s[2]s[3]相同， s[2]s[3] 与 s[4]s[5]相同，s[4]s[5] 与 s[6]s[7] 相同。即s中循环出现最小重复子串。

​			因此，若s的长度是`s'`长度的倍数时，由于根据上述推论，可以推出s会循环出现`s'`的情况，故s可以由`s'`重复组成。

​			当然，若s的长度不是`s'`长度的倍数，s就不会循环出现`s'`，则s就不可以由`s'`重复组成。

##### 参考答案

1. 暴力枚举

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        for i in range(1, n // 2 + 1):
            if n % i == 0:
                if all(s[j] == s[j - i] for j in range(i, n)):
                    return True
        return False
```

​	2. 移动匹配

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        return (s + s).find(s, 1) != len(s)
```

3. **s + s**中使用KMP

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        next = self.getNext(len(s), s)
        tmp = s + s
        j = -1
        for i in range(1, len(tmp) - 1):
            while j >= 0 and tmp[i] != s[j + 1]:
                j = next[j]
            if tmp[i] == s[j + 1]:
                j += 1
            if j == len(s) - 1:
                return True
        return False

    def getNext(self, lenght:int, s:str) -> list:
        j = -1
        next = ["" for _ in range(lenght)]
        next[0] = j
        for i in range(1, lenght):
            while j >= 0 and s[i] != s[j + 1]:
                j = next[j]
            if s[j + 1] == s[i] :
                j += 1
            next[i] = j
        return next
```

4.  优化KMP

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        next = self.getNext(len(s), s)
        t = next[len(s) - 1]
        if t > -1 and len(s) % (len(s) - 1- t) == 0:
            return True
        return False

    def getNext(self, lenght:int, s:str) -> list:
        j = -1
        next = ["" for _ in range(lenght)]
        next[0] = j
        for i in range(1, lenght):
            while j >= 0 and s[i] != s[j + 1]:
                j = next[j]
            if s[j + 1] == s[i] :
                j += 1
            next[i] = j
        return next
```



