### 矩阵旋转
矩阵顺时针转动90度 = 矩阵对角线翻转 + 矩阵`1维`翻转

**代码**
*C++ version*
```
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        auto swap = [&](int i, int j){
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = tmp;
        };
        auto flip = [&](int i, int j){
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[i][n - j - 1];
            matrix[i][n - j - 1] = tmp;
        };
        for(int i = 0; i < n; i++){
            for(int j = i + 1; j < n; j++){
                swap(i, j);
            }
        }
        
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n / 2; j++){
                flip(i, j);
            }
        }
    
        return;
    }
};
```

*python version*
```
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        def swap(i , j):
            tmp = matrix[i][j]
            matrix[i][j] = matrix[j][i]
            matrix[j][i] = tmp
        def flip(i, j):
            tmp = matrix[i][j]
            matrix[i][j] = matrix[i][n - j - 1]
            matrix[i][n - j - 1] = tmp
        n = len(matrix)
        for i in range(n):
            for j in range(i + 1, n):
                swap(i, j)
        for i in range(n):
            for j in range(n // 2):
                flip(i, j)
        return

```