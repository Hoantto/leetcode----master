### 回溯
我们可以通过**跟踪目前为止放置的左括号和右括号的数目**来实现回溯。
- 如果左括号数量不大于$n$，我们可以放一个左括号。如果右括号数量小于左括号数量，我们可以放一个右括号。
- **先放左括号后放右括号的递归顺序**。

**代码**
*C++ version*
```
class Solution {
public:
    void backtrack(vector<string>&res, string&tmp, int open, int close, int n){
        if(tmp.size() == 2 * n){
            res.push_back(tmp);
            return;
        }
        if(open < n){
            tmp.push_back('(');
            backtrack(res, tmp, open+1, close, n);
            tmp.pop_back();
        }
        if(open > close){
            tmp.push_back(')');
            backtrack(res, tmp, open, close+1, n);
            tmp.pop_back();
        }
        return;
    }
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        string tmp;
        backtrack(res, tmp, 0, 0 ,n);
        return res;
    }
};
```
*python version*
```
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        ans = []
        def backtrack(left, right, s):
            if len(s) is 2 * n:
                ans.append("".join(s))
                return
            if left < n:
                s.append('(')
                backtrack(left+1, right, s)
                s.pop()
            if left > right:
                s.append(')')
                backtrack(left, right+1, s)
                s.pop()
        backtrack(0, 0, [])
        return ans
```
