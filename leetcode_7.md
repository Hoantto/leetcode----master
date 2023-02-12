### 数学
假设$rev$为翻转后的数字，重复**弹出**$x$的末尾的数字，将其**推入**$rev$的末尾，直到$x$为$0$。
```
//弹出x的末尾的数字
digit = x % 10
x /= 10
//将数字digit推入rev末尾
rev = rev * 10 + digit
```
题目需要判断反转后的数字是否超过32位有符号整数的范围$[-2^{31},2^{31}-1]$，因此在**推入**数字之前需要判断是否满足：
$$-2^{31}\le rev\cdot 10+digit \le 2^{31}-1$$
若该不等式不成立则返回0。
但是题目要求不允许使用64位整数，因此不能直接按照上诉式子计算。
考虑$x>0$的情况，记$INT\_MAX=2^{31}-1=2147483647$，由于
$$\begin{align}
INT\_MAX& = \lfloor \frac{INT\_MAX}{10} \rfloor \cdot 10 + (INT\_MAX\ mod\ 10)\\
&=\lfloor \frac{INT\_MAX}{10} \rfloor \cdot 10 + 7
\end{align}
$$
则有不等式：
$$rev \cdot 10 + digit \le \lfloor \frac{INT\_MAX}{10} \rfloor \cdot 10 + 7$$
移项得到:
$$(rev - \lfloor \frac{INT\_MAX}{10} \rfloor) \cdot 10 \le 7 - digit$$
该不等式成立的条件：
- 若$rev > \lfloor \frac{INT\_MAX}{10} \rfloor$，由于$digit \ge 0$，不等式不成立。
- 若$rev = \lfloor \frac{INT\_MAX}{10} \rfloor$，当且仅当$digit \le 7$时，不等式成立。
- 若$rev < \lfloor \frac{INT\_MAX}{10} \rfloor$，由于$digit \le 9$，不等式成立。
综上**改判不等式是否成立**：
$$\lceil \frac{-2^{31}}{10} \rceil \le rev \le \lfloor \frac{2^{31}-1}{10} \rfloor$$

**代码**
*C++ version*
```
class Solution {
public:
    int reverse(int x) {
        int rev = 0;
        while(x)
        {
            int pop = x % 10;
            if (rev > INT_MAX/10 || (rev == INT_MAX / 10 && pop > 7)) return 0;
            if (rev < INT_MIN/10 || (rev == INT_MIN / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
            x /= 10;
        }
        return rev;
    }
};
```
*Python version*
```
class Solution:
    def reverse(self, x: int) -> int:
        INT_MIN, INT_MAX = -2**31, 2**31 - 1

        rev = 0
        while x != 0:
  
            if rev < INT_MIN // 10 + 1 or rev > INT_MAX // 10:
                return 0
            digit = x % 10

            if x < 0 and digit > 0:
                digit -= 10

            x = (x - digit) // 10
            rev = rev * 10 + digit
        
        return rev

```
**复杂度分析**
> - 时间复杂度：$O(log|x|)$。
> - 空间复杂度：$O(1)$。
