### 动态规划 or 模拟栈 or 括号特性
#### 动态规划
定义$dp[i]$**表示以下标$i$字符结尾的最长有效括号长度**。初始化$dp$数组为$0$。有效子串一定以$')'$结尾，所以$'('$结尾的子串对应的$dp$的值一定为$0$。只需要求解$')'$在$dp$数组中对应位置的值。

从左向右遍历字符串求解$dp$的值:

1. $s[i]=')'$且$s[i-1]='('$，也就是说字符串形如$"......()"$，我们可以推断出：
$$dp[i]=dp[i-1]+2$$
2. $s[i]=')'$且$s[i-1]=')'$，也就是说字符串形如$".....))"$，我们可以推断出：
    如果$s[i-dp[i-1]-1]='('$，那么：
    $$dp[i]=dp[i-1]+dp[i-dp[i-1]-2]+2$$
如果考虑倒数第二个$)$是一个有效子字符串的一部分，记为$sub_{s}$，如果子字符串$sub_{s}$的前面恰好是$'('$，那么就用$2$加上$sub_{s}$的长度$(dp[i-1])$去更新$dp[i]$。同时，我们也会把有效子字符串$sub_{s}$之前的有效子串的长度加上，也就是再加上$dp[i-dp[i-1]-2]$。

**代码**
*C++ version*
```
class Solution {
public:
    int longestValidParentheses(string s) {
        int max_p = 0, len_s = s.size();
        vector<int> dp(len_s, 0);
        for(int i = 1; i < len_s; i++){
            if(s[i] == ')'){
                if(s[i - 1] == '('){
                    dp[i] = (i >= 2? dp[i - 2]: 0) + 2;
                }else if(i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] == '('){
                    dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2? dp[i - dp[i - 1] - 2]: 0) + 2;
                }
                max_p = max(max_p, dp[i]);
            }
        }
        return max_p;
    }
};
```
*python version*
```
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        max_p, len_s = 0, len(s)
        dp = [0 for _ in range(len_s)]
        for i in range(1, len_s):
            if s[i] == ')':
                if s[i - 1] == '(':
                    dp[i] = (dp[i - 2] if i >= 2 else 0) + 2
                elif i - dp[i - 1] > 0 and s[i - dp[i - 1] - 1] == '(':
                    dp[i] = dp[i - 1] + (dp[i - dp[i - 1] - 2] if i - dp[i - 1] >= 2 else 0) + 2
            max_p = max(max_p, dp[i])
        return max_p
```

#### 模拟栈
我们始终保持**栈底元素**为当前已经遍历过的元素中[最后一个没有被匹配的有括号的下标]，栈里的其他元素维护左括号的下标：
- 对于遇到的每一个$'('$，我们将它的下标放入栈中。
- 对于遇到的每一个$')'$，我们先弹出栈顶元素表示匹配了当前有括号：
    - 如果栈为空，说明当前的右括号为**没有被匹配的右括号**，我们将其下标放入栈中来更新[最后一个没有被匹配的右括号的下标]。
    - 如果栈不为空，当前右括号的下标减去栈顶元素即为[以该右括号为结尾的最长有效括号长度]。
如果栈一开始为空，第一个字符为左括号的时候会将其放入栈中，这样就不满足提及的[最后一个没有被匹配的右括号的下标]在栈底的条件，为了保持统一，我们一开始的时候往栈中放入值为$-1$的元素。

**代码**
*C++ version*
```
class Solution {
public:
    int longestValidParentheses(string s) {
        int max_p = 0;
        stack<int> stk;
        stk.push(-1);
        for(int i = 0; i < s.size(); i++){
            if(s[i] == '('){
                stk.push(i);
            }else{
                stk.pop();
                if(stk.empty()){
                    stk.push(i);
                }else{
                    max_p = max(max_p, i - stk.top());
                }
            }
        }
        return max_p;
    }
};
```

*python version*
```
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        max_p = 0
        stk = [-1]
        for i in range(len(s)):
            if s[i] == '(':
                stk.append(i)
            else:
                stk.pop()
                if len(stk) == 0:
                    stk.append(i)
                else:
                    max_p = max(max_p, i - stk[-1])
        return max_p
```

#### 括号特性 + 贪心
我们利用两个计数器$left$和$right$。首先，从左到右遍历字符串，对于每个$'('$，增加$left$计数器，否则增加$right$计数器。每当$left$计数器和$right$计数器相等时，计算当前有效字符串的长度。当$right$计数器比$left$计数器大时，将$left$和$right$计数器同时变为$0$。

这样做**贪心**地考虑了以当前字符下标结尾的有效括号长度，每当右括号数量对于左括号数量时之前的字符都扔掉不考虑，重新开始下一个字符开始计算，但这样会漏掉一种情况，**遍历的时候左括号数量始终大于右括号的数量**，也即`(()`，这时候无法求解。

我们只需要从右往左遍历用类似的方法计算，这个时候判断条件反了过来：
- 当$left$计数器比$right$计数器大时，我们将$left$和$right$计算器同时变回$0$。
- 当$left$和$right$计数器相等时，计算当前有效字符串的长度。

**代码**
*C++ version*
```
class Solution {
public:
    int longestValidParentheses(string s) {
        int left = 0, right = 0, maxlength = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxlength = max(maxlength, 2 * right);
            } else if (right > left) {
                left = right = 0;
            }
        }
        left = right = 0;
        for (int i = (int)s.length() - 1; i >= 0; i--) {
            if (s[i] == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxlength = max(maxlength, 2 * left);
            } else if (left > right) {
                left = right = 0;
            }
        }
        return maxlength;
    }
};

```

**参考**
- [How to get the last element of a list in Python?](https://www.tutorialspoint.com/How-to-get-the-last-element-of-a-list-in-Python#:~:text=Get%20the%20last%20element%20of%20the%20list%20using%20the%20%E2%80%9Clength,last%20element%20of%20the%20list.)
- [Python List/Array Methods](https://www.w3schools.com/python/python_ref_list.asp)
- [C++ stack api](https://cplusplus.com/reference/stack/stack/)