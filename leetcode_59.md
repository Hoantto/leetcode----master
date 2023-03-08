### 模拟
关键点是使用`visited`矩阵和`total`计数器。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        vector<vector<int>> output(n , vector<int>(n, 0));
        int total = n * n;
        int k = 1;
        int i = 0, j = 0;
        while(total > 0){
            if(!visited[i][j]){
                output[i][j] = k++;
                visited[i][j] = true;
                total--;
            }
            while(j + 1 < n && !visited[i][j + 1]){
                j++;
                output[i][j] = k++;
                visited[i][j] = true;
                total--;
            }
            while(i + 1 < n && !visited[i + 1][j]){
                i++;
                output[i][j] = k++;
                visited[i][j] = true;
                total--;
            }
            while(j - 1 >= 0 && !visited[i][j - 1]){
                j--;
                output[i][j] = k++;
                visited[i][j] = true;
                total--;
            }
            while(i - 1 >= 0 && !visited[i - 1][j]){
                i--;
                output[i][j] = k++;
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
    def generateMatrix(self, n: int) -> List[List[int]]:
        visited = [[False for _ in range(n)] for _ in range(n)]
        output = [[0 for _ in range(n)] for _ in range(n)]
        total = n ** 2
        k = 1
        i ,j = 0 , 0
        while(total > 0):
            if visited[i][j] == False:
                output[i][j] = k
                k += 1
                visited[i][j] = True
                total -= 1
            while j + 1 < n and visited[i][j + 1] == False:
                j += 1
                output[i][j] = k
                k += 1
                visited[i][j] = True
                total -= 1
            while i + 1 < n and visited[i + 1][j] == False:
                i += 1
                output[i][j] = k
                k += 1
                visited[i][j] = True
                total -= 1
            while j - 1 >= 0 and visited[i][j - 1] == False:
                j -= 1
                output[i][j] = k
                k += 1
                visited[i][j] = True
                total -= 1
            while i - 1 >= 0 and visited[i - 1][j] == False:
                i -= 1
                output[i][j] = k
                k += 1
                visited[i][j] = True
                total -= 1
        return output
```