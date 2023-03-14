### 动态规划
最简单的动态规划

**代码**
*C++ version*
```
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
```
*python version*
```
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [1 for _ in range(2)] + [0 for _ in range(n - 1)]
        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[n]
```

### 滚动数组
**代码**
*python version*
```
class Solution:
    def climbStairs(self, n: int) -> int:
        p, q, r = 0, 0, 1
        for i in range(1, n + 1):
            p = q
            q = r
            r = p + q
        return r
```