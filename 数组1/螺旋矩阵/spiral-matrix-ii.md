## 59. 螺旋矩阵

#### 题目描述

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

![image-20221004103747959](C:\Users\南念\AppData\Roaming\Typora\typora-user-images\image-20221004103747959.png)

```python
示例 1：

输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
示例 2：

输入：n = 1
输出：[[1]]
```

**提示**

- `1 <= n <= 20`

##### 思路

1. **定边界法或模拟法**（**主要**）

   **·**  生成一个`n * n`的空矩阵`mat`，随后模拟整个向内环绕的填入过程

   - ​	定义当前左右上下边界`l, r, t, b`， 初始值`num = 1`，迭代值`tar = n * n`

   - 当`num <= tar`时， 始终按照`从左到右` `从上到下` `从右到左` `从下到上`顺序填入循环，每次填入后：

     - 执行`num += 1`: 得到下一个填入数字

     - 更新边界： 

       （1）从左到右填完后，上边界`t += 1`，相当于上边界向内缩1

       （2）从上到下填完后，右边界`r -= 1`， 相当于右边界向内缩1

       （3）从右到左填完后，下边界`b -= 1 `，相当于下边界向内缩1

       （4）从下到上填完后，左边界`l += 1`， 相当于左边界向内缩1

   **· ** 最终返回`mat`即可 
   
   模拟过程如下：
   
   ![image-20221004105713570](C:\Users\南念\AppData\Roaming\Typora\typora-user-images\image-20221004105713570.png)

2.  坚持**循环不变量**原则

   每一圈下来，我们需要画四条边，这四条边怎么画，每画一条边都要坚持一致的左闭右开，或者左开右闭的原则，这样一圈才能按照统一的规则画下来

   例如：按照左闭右开的原则，来画一圈，如下：

   ![image-20221004145440212](C:\Users\南念\AppData\Roaming\Typora\typora-user-images\image-20221004145440212.png)

   这里每一种颜色，代表一条边，遍历的长度，可以看出每一个拐角处的处理规则：拐角处让给新的一条边来继续画。
   
   这坚持了每条边都是**左闭右开**的原则
   
   使用`startx` `starty`变量作为每一圈开始的横纵坐标（对角线上坐标），使用`offset`变量作为每一条边的长度控制，即每条边的长度都比n少offset,初始化为1.
   
   - 从左到右：横坐标恒为startx, 纵坐标为：[starty, n - offset)；
   - 从上到下：横坐标为[startx, n - offset), 纵坐标恒为：n - offset;
   - 从右到左：横坐标恒为n - offset, 纵坐标为：[n - offset, starty);
   - 从下到上：横坐标为[n - offset, startx), 纵坐标恒为：starty；

   每一次循环一圈后，`offset`加一，以减少每一条边的长度。

##### 参考答案

1. 定边界法：

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        ret = [[-1 for i in range(n)] for _ in range(n)]
        t, b, l, r = 0, n - 1, 0, n - 1
        num = 1
        while num <= n ** 2:
            # left to right
            for i in range(l, r + 1):
                ret[t][i] = num
                num += 1
            t += 1
            # top to buttom
            for i in range(t, b + 1):
                ret[i][r] = num
                num += 1
            r -= 1
            # right to left
            for i in range(r, l - 1, -1):
                ret[b][i] = num
                num += 1
            b -= 1
            # bottom to top
            for i in range(b, t - 1, -1):
                ret[i][l] = num
                num += 1
            l += 1
        for i in range(n):
            print(ret[i])
        return ret
```

2. 坚持循环不变量原则

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0] * n for _ in range(n)]
        startx, starty = 0, 0               # 起始点
        loop, mid = n // 2, n // 2          # 迭代次数、n为奇数时，矩阵的中心点
        count = 1                           # 计数

        for offset in range(1, loop + 1) :      # 每循环一层偏移量加1，偏移量从1开始
            for i in range(starty, n - offset) :    # 从左至右，左闭右开
                nums[startx][i] = count
                count += 1
            for i in range(startx, n - offset) :    # 从上至下
                nums[i][n - offset] = count
                count += 1
            for i in range(n - offset, starty, -1) : # 从右至左
                nums[n - offset][i] = count
                count += 1
            for i in range(n - offset, startx, -1) : # 从下至上
                nums[i][starty] = count
                count += 1                
            startx += 1         # 更新起始点
            starty += 1

        if n % 2 != 0 :			# n为奇数时，填充中心点
            nums[mid][mid] = count 
        return nums
```

