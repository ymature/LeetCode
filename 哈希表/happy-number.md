## 202. 快乐数

#### 题目描述

编写一个算法来判断一个数 `n` 是不是快乐数。

**「快乐数」** 定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。
- 如果这个过程 **结果**为 1，那么这个数就是快乐数。

如果 `n` 是 快乐数 就返回 `true` ；不是，则返回 `false` 。

**示例1：**

```pyhton
输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**示例2：**

```python
输入：n = 2
输出：false
```

**提示**：

- `1 <= n <= 231 - 1`

##### 思路

题目中说了不是快乐数就进入了**无限循环**，也就是说**求和的过程中，sum会重复出现**。**这个十分重要**

因此，使用**哈希法**来判断sum是否会**重复出现**，如果重复出现就返回False,否则一直找到sum为1为止。

**set:集合；重要特点：集合中的元素都是唯一的、不重复的，可以用于唯一性、去重复性**

还有一点就是**求和**过程，不断重复求和过程即可。

##### 参考答案

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def calculate(n:int)-> int:
            sum = 0
            while n:
                sum += (n % 10) ** 2
                n = n // 10
            return sum
        
        ret = set()
        sum = calculate(n)
        while sum != 1:
            if sum not in ret:
                ret.add(sum)
                sum = calculate(sum)
            else :
                return False
        return True
```

