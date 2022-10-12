## 54. 螺旋矩阵

#### 题目描述

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

**示例 1：**

![image-20221004155643413](C:\Users\南念\AppData\Roaming\Typora\typora-user-images\image-20221004155643413.png)

`输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]`
`输出：[1,2,3,6,9,8,7,4,5]`
示例 2：

![image-20221004155720690](C:\Users\南念\AppData\Roaming\Typora\typora-user-images\image-20221004155720690.png)

`输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]`
`输出：[1,2,3,4,8,12,11,10,9,5,6,7]`

**提示**：

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

##### 思路

1. **定边界法**（**主要**）
2. **循环不变量原则**

##### 参考答案

1. 

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        ret = []
        t, b, l, r = 0, len(matrix) - 1, 0, len(matrix[0]) - 1
        num = 1
        while num <= len(matrix) * len(matrix[0]):
            # left to right
            for i in range(l, r + 1):
                ret.append(matrix[t][i])
                num += 1
            t += 1
            # top to bottom
            for i in range(t, b + 1):
                ret.append(matrix[i][r])
                num += 1
            r -= 1
            # right to left
            for i in range(r, l - 1, -1):
                ret.append(matrix[b][i])
                num += 1
            b -= 1
            # bottom to top
            for i in range(b, t - 1, -1):
                ret.append(matrix[i][l])
                num += 1
            l += 1
        return ret[:len(matrix) * len(matrix[0])]
```

2. 

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        ret = []
        startx = starty = 0
        n = len(matrix)
        m = len(matrix[0])
        midn = n // 2
        midm = m // 2 
        loop = min(n, m) // 2
        offset = 0
        for offset in range(1, loop + 1):
            for i in range(starty, m - offset):
                ret.append(matrix[startx][i])
            for i in range(startx, n - offset):
                ret.append(matrix[i][m - offset])
            for i in range(m - offset, starty, -1):
                ret.append(matrix[n - offset][i])
            for i in range(n - offset, startx, -1):
                ret.append(matrix[i][starty])
            startx += 1
            starty += 1
        # 当n != m时，由于最后剩下一行/一列未被访问，注意不够一圈时，**offset = 0**; **m - offset - 1位于[0, m - 1]之内**
        if m > n:
           for i in range(starty, m - offset):
                ret.append(matrix[startx][i])
        elif m < n:
            for i in range(startx, n - offset):
                ret.append(matrix[i][m - offset - 1])
        else:
            if n % 2 != 0:
                ret.append(matrix[midn][midn])
        return ret[:n * m]
```

