### 动态规划
注意：**动态规划题目应该只专注于转移方程的构建，不要东想西想**

首先可以将所有操作归纳为以下三个操作：
- 在单词`A`中插入一个字符
- 在单词`B`中插入一个字符
- 修改单词`A`中的一个字符

用`D[i][j]`表示`A`的前`i`个字符和`B`的前`j`个字符之间的编辑距离
- `D[i][j - 1]`为`A`的前`i`个字符和`B`的前`j - 1`个字符的编辑距离问题。对于`B`的第`j`个字符，在`A`的末尾添加一个相同的字符，那么`D[i][j]`最小可以表示为`D[i][j - 1] + 1`
- `D[i - 1][j]`为`A`的前`i - 1`个字符和`B`的前`j`个字符的编辑距离问题。对于`A`的第`i`个字符，在`B`的末尾添加一个相同的字符，那么`D[i][j]`最小可以表示为`D[i - 1][j] + 1`
- `D[i - 1][j - 1]`为`A`的前`i - 1`个字符和`B`的前`j - 1`个字符的编辑距离问题。即对于`B`的第`j`个字符，修改`A`的第`i`个字符使它们相同，那么`D[i][j]`最小可以为`D[i-1][j-1] + 1`。特别地，如果`A`的第`i`个字符和`B`的第`j`个字符原本就相同，那么实际上不需要进行修改操作。在这种情况下，`D[i][j]`最小可以为`D[i-1][j-1]`。

**代码**
*C++ version*
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size();
        int m = word2.size();
        // 有一个字符串为空
        if(n * m == 0) return n + m;
        vector<vector<int>> dp(n + 1, vector<int>(m + 1));
        //初始化
        for(int i = 0; i < n + 1; i++){
            dp[i][0] = i;
        }
        for(int j = 0; j < m + 1; j++){
            dp[0][j] = j;
        }
        for(int i = 1; i < n + 1; i++){
            for(int j = 1; j < m + 1; j++){
                int add_a = dp[i][j - 1] + 1;
                int add_b = dp[i - 1][j] + 1;
                int change = dp[i - 1][j - 1];
                if(word1[i - 1] != word2[j - 1]) change++;
                dp[i][j] = min(add_a, min(add_b, change));
            }
        }
        return dp[n][m];
    }
};
```

*python version*
```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        n, m = len(word1), len(word2)
        if n * m == 0:
            return n + m
        dp = [[0 for _ in range(m + 1)] for _ in range(n + 1)]
        for i in range(n + 1):
            dp[i][0] = i
        for j in range(m + 1):
            dp[0][j] = j
        for i in range(1, n + 1):
            for j in range(1, m + 1):
                add_a = dp[i][j - 1] + 1
                add_b = dp[i - 1][j] + 1
                change = dp[i - 1][j - 1]
                if word1[i - 1] != word2[j - 1]:
                    change += 1
                dp[i][j] = min(add_a, min(add_b, change))
        return dp[n][m]
      
```