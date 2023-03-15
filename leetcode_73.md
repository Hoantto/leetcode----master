### 哈希表
#### method1
分别记录每一行、每一列是否有零出现。

**代码**
*python version*
```
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        m, n = len(matrix), len(matrix[0])
        row, col = [False] * m, [False] * n

        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:
                    row[i] = col[j] = True
        
        for i in range(m):
            for j in range(n):
                if row[i] or col[j]:
                    matrix[i][j] = 0

```

#### method2

可以用矩阵的第一行和第一列代替方法一中的两个标记数组，以达到$O(1)$的额外空间。但这样会导致原数组的第一行和第一列被修改，无法记录它们是否原本包含0。因此我们需要额外使用两个标记变量分别记录第一行和第一列是否原本包含0。

在实际代码中，首先预处理出两个标记变量，**接着使用其他行与列去处理第一行与第一列，然后反过来使用第一行与第一列去更新其他行与列，最后使用两个标记变量更新第一行与第一列即可**。

**代码**
*python version*
```
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        m , n = len(matrix), len(matrix[0])
        flag_col0 = any(matrix[i][0] == 0 for i in range(m))
        flag_raw0 = any(matrix[0][j] == 0 for j in range(n))
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == 0:
                    matrix[i][0] = matrix[0][j] = 0
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0
        if flag_col0:
            for i in range(m):
                matrix[i][0] = 0
        if flag_raw0:
            for j in range(n):
                matrix[0][j] = 0
        return matrix
        
```

这道题不能用``numpy`的思维去做`whole raw or col slice replacement`，比如`matrix[:][j] = [0] * m`。

**参考**
- [Python any() Function](https://www.w3schools.com/python/ref_func_any.asp)
- [How do I change column of a 2d list in Python without nuumpy?](https://stackoverflow.com/questions/47985788/how-do-i-change-column-of-a-2d-list-in-python)