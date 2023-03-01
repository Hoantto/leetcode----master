### 动态规划
令`dp[i][j]`代指`s`的前$i$个字符和`p`的前$j$个字符匹配。当`p[j - 1] = '*`时，可以选择和`s[i - 1]`匹配或者不匹配。当`p[j - 1]='?`或者`s[i - 1] = p[i - 1]`时，`dp[i][j] = dp[i - 1][j - 1]`。

状态转移方程为：
$$
dp[i][j]=
\begin{cases}
dp[i - 1][j]\ |\ dp[i][j - 1] &\text{p[j - 1]='*'}\\
dp[i - 1][j - 1] &\text{p[j - 1]='?'\ or\ s[i - 1]=p[j - 1]}
\end{cases}
$$

初始条件为：
- $dp[0][0]=True$
- $dp[i][0]=False$
- $dp[0][j]$需要分类讨论只有当$p$的前$j$个字符为星号时，$dp[0][j]$才为真

**代码**
*C++ version*
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        dp[0][0] = true;
        for (int i = 1; i <= n; ++i) {
            if (p[i - 1] == '*') {
                dp[0][i] = true;
            }
            else {
                break;
            }
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (p[j - 1] == '*') {
                    dp[i][j] = dp[i][j - 1] | dp[i - 1][j];
                }
                else if (p[j - 1] == '?' || s[i - 1] == p[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
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
    def isMatch(self, s: str, p: str) -> bool:
        m, n = len(s), len(p)
        dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
        dp[0][0] = True
        for i in range(1, n + 1):
            if p[i - 1] == '*':
                dp[0][i] = True
            else:
                break
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if p[j - 1] == '*':
                    dp[i][j] = dp[i - 1][j] | dp[i][j - 1]
                elif p[j - 1] == '?' or s[i - 1] == p[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
        return False if dp[m][n] == 0 else True
```