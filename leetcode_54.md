### 数组模拟
本题的关键在于需要使用`visited`数组与`total`计数器来控制循环。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<bool>> visited;
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> output;
        int m = matrix.size();
        int n = matrix[0].size();
        visited.resize(m, vector<bool>(n, false));
        int i = 0, j = 0;
        int total = m * n;
        while(total > 0){
            if(!visited[i][j]){
                output.push_back(matrix[i][j]);
                visited[i][j] = true;
                total--;
            }
            while(j + 1 < n && !visited[i][j + 1]){
                output.push_back(matrix[i][j + 1]);
                j++;
                visited[i][j] = true;
                total--;
            }
            while(i + 1 < m && !visited[i + 1][j]){
                output.push_back(matrix[i + 1][j]);
                i++;
                visited[i][j] = true;
                total--;
            }
            while(j - 1 >= 0 && !visited[i][j - 1]){
                output.push_back(matrix[i][j - 1]);
                j--;
                visited[i][j] = true;
                total--;
            }
            while(i - 1 >= 0 && !visited[i - 1][j]){
                output.push_back(matrix[i - 1][j]);
                i--;
                visited[i][j] = true;
                total--;
            }
        }
        return output;
    }
};
```

*python version*
```
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        m ,n = len(matrix), len(matrix[0])
        visited = [[False for _ in range(n)] for _ in range(m)]
        i, j = 0, 0
        total = m * n
        output = []
        while total > 0:
            if visited[i][j] == False:
                visited[i][j] = True
                output += [matrix[i][j]]
                total -= 1
            while j + 1 < n and visited[i][j + 1] == False:
                j += 1
                output += [matrix[i][j]]
                visited[i][j] = True
                total -= 1
            while i + 1 < m and visited[i + 1][j] == False:
                i += 1
                output += [matrix[i][j]]
                visited[i][j] = True
                total -= 1
            while j - 1 >= 0 and visited[i][j - 1] == False:
                j -= 1
                output += [matrix[i][j]]
                visited[i][j] = True
                total -= 1
            while i - 1 >= 0 and visited[i - 1][j] == False:
                i -= 1
                output += [matrix[i][j]]
                visited[i][j] = True
                total -= 1
        return output
```