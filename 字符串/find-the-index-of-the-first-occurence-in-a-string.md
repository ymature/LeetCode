## 28. 找出字符串中第一个匹配项的下标

#### 题目描述

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

**示例1**：

```python
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 
```

**示例2：**

```python
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

**提示**：

- `1 <= haystack.length, needle.length <= 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

##### 思路

1. 使用`python`进行窗口逐一比较
2. 使用`KMP`算法（经典题型）

##### KMP算法的介绍

###### KMP算法有什么用

​	KMP主要应用于**字符串匹配**

​	主要思想：**当出现字符串不匹配时，可以知道一部分之前已经匹配的文本内容，可以利用这些信息避免从头再去做匹配了。**

###### 什么是前缀表

​	next数组就是一个前缀表

​	前缀表的作用：**前缀表是用来回退的，它记录了模式串与主串(文本串)不匹配的时候，模式串应该从哪里开始重新匹配。**

​	作用过程如下：

![image-20221017105353454](https://code-thinking.cdn.bcebos.com/gifs/KMP%E7%B2%BE%E8%AE%B21.gif)

​	可以看到，当文本串中第六个字符`b`和模式串中的`f`不匹配时，如果使用暴力匹配就需要从头匹配。但如果使用前缀表就不会从头匹配，而是从上次已经匹配的内容开始匹配，如上过程中就找到了模式串中的`b`继续开始匹配，**而不需要从头开始匹配**

​	什么是前缀表：**记录下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。**

###### 最长公共前后缀

​	前缀：**不包含最后一个字符的所有以第一个字符开头的连续子串**。

​	后缀：**不包含第一个字符的所有以最后一个字符结尾的连续子串**

​	最长公共前后缀：**最大相同前后缀的长度**

###### 为什么要使用前缀表

​	使用前缀表就不需要从头开始匹配。

###### 如何计算前缀表

​	前缀表中的值：**下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。**

​	例如：

![image-20221017105353454](https://code-thinking.cdn.bcebos.com/pics/KMP%E7%B2%BE%E8%AE%B25.png)

![image-20221017105353454](https://code-thinking.cdn.bcebos.com/pics/KMP%E7%B2%BE%E8%AE%B26.png)

​	第一个`a`中最长相同前后缀的长度为0，因此，前缀表的第零个值是：0；

​	第二个`aa`中最长相同前后缀的长度为1，因此，前缀表的第一个值是：1；

​	最后结果如下：
![image-20221017105353454](https://code-thinking.cdn.bcebos.com/pics/KMP%E7%B2%BE%E8%AE%B28.png)

###### 如何使用前缀表

​	如下所示	

![image-20221017105353455](https://code-thinking.cdn.bcebos.com/gifs/KMP%E7%B2%BE%E8%AE%B22.gif)

​	找到不匹配的位置，此时需要看它的前一个字符的前缀表是多少，**根据`前一个字符`的前缀表数值来移动模式串的匹配指针**

###### 前缀表与next数组

​	**next数组可以是前缀表，也可以是前缀表减一**，与KMP算法的原理无关，只是与具体的实现有关

###### 构造next数组

​	**以next数组是前缀表减一为例**

​	其实就是求模式串的前缀表的过程，主要有三步：

- 初始化
- 处理前后缀不相同的情况
- 处理前后缀相同的情况

 1. 初始化

    定义两个指针`i,j`，`j`指向前缀末尾位置，`i`指向后缀末尾位置。

    进行next数组初始化，如下：

    ```C++
    int j = -1;
    next[0] = j;
    ```

    j初始化为`-1`,是因为next数组使用了前缀表减一为例

    next[i]表示 i（包括i）之前最长相等的前后缀长度减一（其实就是j）

2. 处理前后缀不相同的情况

   由于next[0]已经初始化，因此，i从1开始求出模式串的next数组

   由于j指向前缀末尾位置，而i指向待求next[i]的位置(即后缀新加入的字符)，因此需要进行比较的是s[i]和s[j+1]（前缀新加入的字符）
   
   若两者不相同，则将`j = next[j]`，理由如下：
   由于j指向的是next[i - 1]，即j指向包括`i - 1`在内的最长公共前后缀中的前缀的末尾位置，因此：
   
   ![image-20221018092512502](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221018092512502.png)

​        图中红色和蓝色区域分别是最长公共前后缀，两个区域相同。

​		故，`s[j + 1] != s[i] ` 时，

​		![image-20221018093153141](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221018093153141.png)

 	    上图中绿色和紫色的区域相同，又由于红色和蓝色区域相同，导致结果如下：
		![image-20221018093357886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221018093357886.png)

​		上图三种颜色的区域都相同，因此，`s[j + 1] != s[i] ` 时，`j = next[j]`,直到`s[j + 1] = s[i]`，使得包含`i`在内的子串找到最大公共前后缀。

​		代码如下：

```C++
while (j >= 0 && s[i] != s[j + 1]) { // 前后缀不相同了
    j = next[j]; // 向前回退
}
```

 3. 处理前后缀相同的情况

    如果`s[i] = s[j + 1]`,说明找到了最长公共前后缀，将`j++`，指向包含i在内的最长公共前后缀中的前缀末尾。使得`next[i] = j `。代码如下：
    ```C++
    if (s[i] == s[j + 1]) { // 找到相同的前后缀
        j++;
    }
    next[i] = j;
    ```

    最后构造next数组的函数代码如下：

    ```c++
    void getNext(int* next, const string& s){
        int j = -1;
        next[0] = j;
        for(int i = 1; i < s.size(); i++) { // 注意i从1开始
            while (j >= 0 && s[i] != s[j + 1]) { // 前后缀不相同了
                j = next[j]; // 向前回退
            }
            if (s[i] == s[j + 1]) { // 找到相同的前后缀
                j++;
            }
            next[i] = j; // 将j（前缀的长度）赋给next[i]
        }
    }
    ```

###### 使用next数组进行匹配

​	构造好next数组之后，使用双指针`i,j`；`i`指向文本串的匹配位置，`j`指向模式串的匹配位置的前一个，故将`t[i]`与`s[j + 1]`进行比较。

​	当`j`指向模式串的末尾位置时，说明模式串匹配成功。

```c++
int j = -1; // 因为next数组里记录的起始位置为-1
for (int i = 0; i < s.size(); i++) { // 注意i就从0开始
    while(j >= 0 && s[i] != t[j + 1]) { // 不匹配
        j = next[j]; // j 寻找之前匹配的位置
    }
    if (s[i] == t[j + 1]) { // 匹配，j和i同时向后移动
        j++; // i的增加在for循环里
    }
    if (j == (t.size() - 1) ) { // 文本串s里出现了模式串t
        return (i - t.size() + 1);
    }
}
```

##### 参考答案

1.

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if len(haystack) < len(needle):
            return -1
        slow = fast = 0
        for _ in range(len(needle)):
            fast += 1
        while fast <= len(haystack) and haystack[slow:fast] != needle:
            slow += 1
            fast += 1
        return slow if haystack[slow:fast] == needle else -1
```

2.  next数组是前缀表减一的情况

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if len(haystack) < len(needle):
            return -1
        a = len(haystack)
        b = len(needle)
        next = self.getNext(b, needle)
        j = -1
        for i in range(a):
            while j >= 0 and haystack[i] != needle[j + 1]:
                j = next[j]
            if haystack[i] == needle[j + 1]:
                j += 1
            if j == b - 1:
                return i - b + 1
        return -1
        
    def getNext(self, length:int, needle:str) -> list:
        j = -1
        next = ['' for _ in range(length)]
        next[0] = j
        for i in range(1, length):
            while j >= 0 and needle[i] != needle[j + 1]:
                j = next[j]
            if needle[i] == needle[j + 1]:
                j += 1
            next[i] = j
        return next
```

3. next数组是前缀表的情况

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        a=len(needle)
        b=len(haystack)
        if a==0:
            return 0
        i=j=0
        next=self.getnext(a,needle)
        while(i<b and j<a):
            if j==-1 or needle[j]==haystack[i]:
                i+=1
                j+=1
            else:
                j=next[j]
        if j==a:
            return i-j
        else:
            return -1

    def getnext(self,a,needle):
        next=['' for i in range(a)]
        j,k=0,-1
        next[0]=k
        while(j<a-1):
            if k==-1 or needle[k]==needle[j]:
                k+=1
                j+=1
                next[j]=k
            else:
                k=next[k]
        return next
```

