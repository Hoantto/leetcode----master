### 动态规划
本题只需要对第一行、第一列的`dp`特殊处理，并对存在障碍物的网格`dp[i][j] = 0`即可(**意味着此路不通**)。

**代码**
*C++ version*
```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        bool has_ob = false;
        for(int i = 1; i <= m; i++){
            if(obstacleGrid[i- 1][0] == 1){
                has_ob = true;
            }
            if(has_ob){
                break;
            }
            dp[i][1] = 1;
        }
        has_ob = false;
        for(int j = 1; j <= n; j++){
            if(obstacleGrid[0][j - 1] == 1){
                has_ob = true;
            }
            if(has_ob){
                break;
            }
            dp[1][j] = 1; 
        }
        for(int i = 2; i <= m; i++){
            for(int j = 2; j <= n; j++){
                if(obstacleGrid[i - 1][j - 1] == 1){
                    dp[i][j] = 0;
                }else{
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        return dp[m][n];
    }
};
```

*python version*
```
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
        has_ob = False
        for i in range(1, m + 1):
            if obstacleGrid[i - 1][0] == 1:
                has_ob = True
            if has_ob == True:
                break
            dp[i][1] = 1
        has_ob = False
        for j in range(1, n + 1):
            if obstacleGrid[0][j - 1] == 1:
                has_ob = True
            if has_ob == True:
                break
            dp[1][j] = 1
        for i in range(2, m + 1):
            for j in range(2, n + 1):
                if obstacleGrid[i - 1][j - 1] == 1:
                    dp[i][j] = 0
                else:
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[m][n]
```
