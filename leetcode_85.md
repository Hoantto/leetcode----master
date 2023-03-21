### 单调栈
此题为单调栈的经典题目，我们首先计算出矩阵的左边连续`1`的数量，使用二维数组`left`记录，其中`left[i][j]`为矩阵第`i`行第`j`元素的左边连续`1`的数量。然后对于每一列做`leetcode_84`的最大矩形计算即可。

**代码**
*C++ version*
```
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int m = matrix.size();
        if(m == 0) return 0;
        int n = matrix[0].size();
        vector<vector<int>> left(m, vector<int>(n, 0));
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(matrix[i][j] == '1'){
                    left[i][j] = (j == 0? 0: left[i][j - 1]) + 1;
                }
            }
        }
        int ans = 0;
        for(int j = 0; j < n; j++){
            vector<int> up(m, -1);
            vector<int> down(m, m);
            stack<int> stk;
            for(int i = 0; i < m; i++){
                while(!stk.empty() && left[stk.top()][j] >= left[i][j]){
                    down[stk.top()] = i;
                    stk.pop(); 
                }
                up[i] = (stk.empty()? -1: stk.top());
                stk.push(i);
            }
            for(int i = 0; i < m; i++){
                ans = max(ans, (down[i] - up[i] - 1)*left[i][j]);
            }
        }
        return ans;
    }
};
```

*python version*
```
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        m = len(matrix)
        if m == 0:
            return 0
        n = len(matrix[0])
        left = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == "1":
                    left[i][j] = (0 if j == 0 else left[i][j - 1]) + 1
        ans = 0
        for j in range(n):
            up = [-1 for _ in range(m)]
            down = [m for _ in range(m)]
            stk = []
            for i in range(m):
                while len(stk) != 0 and left[stk[-1]][j] >= left[i][j]:
                    down[stk[-1]] = i
                    stk.pop()
                up[i] = -1 if len(stk) == 0 else stk[-1]
                stk += [i]
            for i in range(m):
                ans = max(ans, (down[i] - up[i] - 1)*left[i][j])
        return ans
```