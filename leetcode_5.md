### 动态规划
用$P(i,j)$表示字符串的第$i$到第$j$个字母组成的串($s[i:j]$)是否为回文串：
$$P(i,j)=
\begin{cases}
true, &\text{如果子串$S_i...S_j$是回文串}\\
false, &\text{其他情况}
\end{cases}$$
这里的[其他情况]包含两种可能性：
- $s[i,j]$本身不是一个回文串；
- $i>j$，此时$s[i,j]$本身不合法。
状态转移方程为：$$P(i,j)=P(i+1,j-1)\land(S_i==S_j)$$
其中边界条件为：$$
\begin{cases}
P(i,i)=true\\
P(i,i+1)=(S_i==S_{i+1})
\end{cases}
$$
**注意：在状态转移方程中，是从长度较短的字符串向长度较长的字符串进行转移的，要注意动态规划的循环顺序。**

**代码**
*C++ version*
```
class Solution {
public:
    string longestPalindrome(string s) {
        int s_len = s.size();
        vector<vector<bool>> dp(s_len, vector<bool>(s_len, false));
        int start = 0, max_size = 1;
        for(int i = 0; i < s_len; i++){
            dp[i][i] = true;
            if(i + 1 < s_len){
                if(s[i] == s[i + 1]){
                    dp[i][i + 1] = true;
                    start = i;
                    max_size = 2;
                }
            }
        }
        for(int len = 3; len <= s_len; len++){
            for(int i = 0; i <= s_len - len; i++){
                int j = i + len - 1;
                if(s[i] == s[j] && dp[i + 1][j - 1] == true){
                    dp[i][j] = true;
                    start = i;
                    max_size = len;
                }
            }
        }
        return s.substr(start, max_size);
    }
};
```

*python version*
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        if n < 2:
            return s
        
        max_len = 1
        begin = 0
        dp = [[False] * n for _ in range(n)]
        for i in range(n):
            dp[i][i] = True
        for L in range(2, n + 1):
            for i in range(n):
                j = L + i - 1
                if j >= n:
                    break
                    
                if s[i] != s[j]:
                    dp[i][j] = False 
                else:
                    if j - i < 3:
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i + 1][j - 1]
                
                if dp[i][j] and j - i + 1 > max_len:
                    max_len = j - i + 1
                    begin = i
        return s[begin:begin + max_len]


```
**复杂度分析**
> - 时间复杂度：$O(n^2)$，$n$为字符串的长度。
> - 空间复杂度：$O(n^2)$。