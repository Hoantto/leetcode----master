### 动态规划 or 正则表达式
#### 动态规划method
题目中的匹配是一个**逐步匹配**的过程，用$f[i][j]$表示$s$的前$i$个字符与$p$中前$j$个字符是否能够匹配：
- 如果$p$的第$j$个字符是一个小写字母，那么必须在$s$中匹配一个相同的小写字母：
    $$
    f[i][j]=
    \begin{cases}
    f[i-1][j-1], &\text{s[i] = p[j]}\\
    false, &\text{s[i]$\ne$p[j]}
    \end{cases}
    $$
    也就是说，如果$s$的第$i$个字符与$p$的第$j$个字符不相同，那么无法进行匹配；否则可以匹配两个字符串的最后一个字符，完整的匹配结果取决于两个字符串前面的部分。
- 如果$p$的第$j$个字符是`*`，那么就表示可以对$p$的第$j-1$个字符匹配任意自然的次数，在$0$次匹配下，有：
    $$f[i][j]=f[i][j-2]$$
    也就是说**浪费**了一个字符+星号的组合，在匹配$1,2,3,...$次的情况下，类似地有：
    $$
    \begin{align}
    &f[i][j]=f[i-1][j-2],\ if\ s[i]=p[j-1]\\
    &f[i][j]=f[i-2][j-2],\ if\ s[i-1]=s[i]=p[j-1]\\
    &f[i][j]=f[i-3][j-2],\ if\ s[i-2]=s[i-1]=s[i]=p[j-1]\\
    &......
    \end{align}
    $$
    这种编码很麻烦，考虑另一种匹配过程，本质上会有两种情况：
    - 匹配$s$末尾的一个字符，将该字符扔掉，该组合还可以继续进行匹配。
    - 不匹配字符，将该组合扔掉，不再进行匹配。
    状态转移方程为：
    $$
    f[i][j]=
    \begin{cases}
    f[i-1][j]\ or\ f[i][j-2], &\text{s[i] = p[j-1]}\\
    f[i][j-2],  &\text{s[i]$\ne$p[j-1]}
    \end{cases}
    $$
- 在任意情况下，只要$p[j]$是`.`，那么$p[j]$一定能成功匹配$s$中任意一个小写字符。
最终的转移方程如下：
$$
f[i][j]=
\begin{cases}
if\ (p[j]\ne'*')=
    \begin{cases}
    f[i-1][j-1],    &\text{matches(s[i],p[j])}\\
    false,  &\text{otherwise}
    \end{cases}\\
otherwise=
    \begin{cases}
    f[i-1][j]\ or\ f[i][j-2],   &\text{matches(s[i],p[j-1])}\\
    f[i][j-2],  &\text{otherwise}
    \end{cases}
\end{cases}
$$

**细节**
动态规划的边界条件为$f[0][0]=true$，也即；两个空字符串是可以匹配的。最终的答案为$f[m][n]$，其中$m$和$n$分别是字符串$s$和$p$的长度。

**代码**
*C++ version*
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();

        auto matches = [&](int i, int j){
            if(i == 0) return false;
            if(p[j - 1]=='.') return true;
            return s[i - 1] == p[j - 1];
        };

        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        f[0][0] = true;
        for(int i = 0; i <= m; i++){
            for(int j = 1; j <=n; j++){
                if(p[j - 1] == '*'){
                    f[i][j] |= f[i][j - 2];
                    if(matches(i, j - 1)){
                        f[i][j] |= f[i - 1][j];
                    }
                }else{
                    if(matches(i, j)){
                        f[i][j] |= f[i - 1][j - 1];
                    }
                }
            }
        }
        return f[m][n];
    }
};
```
**复杂度分析**
> - 时间复杂度：$O(mn)$。
> - 空间复杂度：$O(mn)$。
#### 正则表达式method
**代码**
*python version*
```
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        if(re.fullmatch(p, s) != None):
            return True
        else:
            return False      
```
**参考**
- [python re api](https://docs.python.org/3/library/re.html)
- [python string replace method](https://www.w3schools.com/python/ref_string_replace.asp)

