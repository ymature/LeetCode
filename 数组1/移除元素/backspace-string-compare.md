## 844. 比较含退格的字符串

#### 题目描述

给定 s 和 t 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 true 。# 代表退格字符。

**注意**：如果对空文本输入退格字符，文本继续为空

```python
示例 1：

输入：s = "ab#c", t = "ad#c"
输出：true
解释：s 和 t 都会变成 "ac"。
示例 2：

输入：s = "ab##", t = "c#d#"
输出：true
解释：s 和 t 都会变成 ""。
示例 3：

输入：s = "a#c", t = "b"
输出：false
解释：s 会变成 "c"，但 t 仍然是 "b"。
```

**提示**

- `1 <= s.length, t.length <= 200`
- `s` 和 `t` 只含有小写字母以及字符 `'#'`

##### 思路：

1.  删除元素（快慢指针）**空间复杂度高**
   - 快指针遇到`#`时，慢指针向后移一格，（**慢指针到达零位置处时，不在向后移动**）快指针仍向前移动
   - 快指针遇到非`#`元素时，将快指针元素复制给慢指针元素，之后同时向前移一格

2. **双指针逆序遍历**（**新思路**）

   由于`#`号只会消除左边的一个字符，对右边的字符没有影响，所以可以选择**从后往前**遍历S，T字符串。

   1. 准备两个指针i, j分别指向S，T的末尾字符，再准备两个变量skipS，skipT分别存放S，T字符串中当前`#`数量
   2. 从后往前遍历S，情况有三：
      -  若当前字符为`#`，则skipS自增1
      -  若当前字符不是`#`，且skipS不为0，则说明还有`#`没有进行删除功能；因此，i指针i--，skipS自减1；
      -  若当前字符不是`#`，且skipS为0，则说明当前字符不会被删除，可以用来和T中的当前字符进行比较。

​	若对比过程中出现S、T当前字符不匹配，则遍历结束，返回false，若S、T均遍历结束，且可以一一匹配，返回true。

##### 参考答案

1. ```python
   class Solution:
       def backspaceCompare(self, s: str, t: str) -> bool:
           def getString(s:str)->str:
               sindex = 0
               for element in s:
                   if element == "#" and sindex > 0:
                       sindex -= 1
                   elif element != '#':
                       s = s[:sindex] + str(element) + s[sindex+1:]
                       sindex += 1
               return s[:sindex]
           s = getString(s)
           t = getString(t)
           return s == t
   ```

2. ```python
   class Solution:
       def backspaceCompare(self, s: str, t: str) -> bool:
           i, j = len(s) - 1, len(t) - 1
           skipS, skipT = 0, 0
           while i >= 0 or j >= 0:
               # 寻找可比较字符
               while i >= 0:
                   if s[i] == '#':
                       skipS += 1
                       i -= 1
                   elif skipS > 0:
                       i -= 1
                       skipS -= 1
                   else :
                       break
               while j >= 0:
                   if t[j] == '#':
                       skipT += 1
                       j -= 1
                   elif skipT > 0:
                       j -= 1
                       skipT -= 1
                   else:
                       break
               # s[i]和t[j]均是字符，可以比较
               if i >= 0 and j >= 0:
                   if s[i] != t[j]:
                       return False
               # i>=0 or j >= 0 说明有一方的值小于零，而另一方的值大于等于零，即一方已没有可比较的字符，而另一方还存在可比较字符，故返回false。
               elif i >= 0 or j >= 0:
                   return False
               i -= 1
               j -= 1
           return True            
   ```