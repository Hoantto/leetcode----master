### 动态规划 + 记忆化搜索(自顶向下)
设$f(s_1,s_2)$表示$s_1$和$s_2$是否[和谐]，那么我们可以写出状态转移方程：
$$
f(s_1,s_2)=
\begin{cases}
True, &\ s_1=s_2\\
False, &\ 存在某个字符，他在s_1和s_2中的出现次数不同
\end{cases}
$$
因为题目保证给定的原始字符串的长度相同，因此我们只需要判断上面的两个情况。如果$s_1$和$s_2$不符合上面的两种情况，那么我们需要枚举分割点。

设$s_1$和$s_2$的长度为$n$，我们用$s_1(x,y)$表示$s_1$从第$x$个字符（从0开始编号）开始，长度为$y$的子串。由于分割出的两个字符串不能为空串，那么其中一个字符串就是$s_1(0,i)$，另一个字符串是$s_2(i,n-i)$。
- 对于$l(s_1)$和$r(s_1)$没有被交换的情况，$s_2$童颜需要被分为$s_2(0,i)$以及$s_2(i,n-i)$，否则长度不同的字符串是不可能[和谐]的。因此我们可以写出状态转移方程：
$$f(s_1,s_2)=\bigvee_{i=1}^{n-1}(f(s_1(0,i),s_2(0,i))\land f(s_1(i,n-i),s_2(i,n-i)))$$

- 对于$l(s_1)$和$r(s_1)$被交换的情况，$s_2$需要被分为$s_2(0,n-i)$以及$s_2(n-i,i)$，这样对应的长度才相同。因此我们可以写出状态转移方程：
$$f(s_1,s_2)=\bigvee_{i=1}^{n-1}(f(s_1(0,i),s_2(n-i,i))\land f(s_1(i,n-i),s_2(0,n-i)))$$
将上面两种状态转移方程用$\lor$或运算拼在一起，即可得到最终的状态转移方程。

**技巧**
> 使用[记忆化搜索]自顶向下地进行动态规划，递归地计算所有的$f$值
> 改变子串的表示方式，优化传参，将状态变为$f(i_1,i_2,length)$，此时只需要在递归时传递三个整数类型的变量，省去了字符串的操作

**代码**
*python version*
```
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        @cache
        def dfs(i1: int, i2: int, len: int) -> bool:
            if s1[i1:i1 + len] == s2[i2:i2 + len]:
                return True
            if Counter(s1[i1:i1 + len]) != Counter(s2[i2:i2 + len]):
                return False
            for i in range(1, len):
                if dfs(i1, i2, i) and dfs(i1 + i, i2 + i, len - i):
                    return True
                if dfs(i1, i2 + len - i, i) and dfs(i1 + i, i2, len - i):
                    return True
            return False
        ans = dfs(0, 0, len(s1))
        dfs.cache_clear()
        return ans
```

**参考**
- [funtools.cache](https://docs.python.org/3/library/functools.html)
- [Counter objects](https://docs.python.org/3/library/collections.html)
