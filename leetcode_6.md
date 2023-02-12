**常规模拟**
**代码**
*C++ version*
```
class Solution {
public:
    string convert(string s, int numRows) {
        int len = s.size();
        if(len <= numRows) return s;
        if(numRows == 1) return s;
        vector<string> n(min(len, numRows));
        int reverse = -1;
        int cur_row = 0;
        for(char c:s){
            n[cur_row] += c;
            if(cur_row == 0 || cur_row == numRows - 1){
                reverse = -reverse;
            }
            cur_row += reverse;
        }
        string ret;
        for(string s:n){
            ret += s;
        }
        return ret;
    }
};
```
*python version*
```
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows < 2: return s
        res = ["" for _ in range(numRows)]
        i, flag = 0, -1
        for c in s:
            res[i] += c
            if i == 0 or i == numRows - 1: flag = -flag
            i += flag
        return "".join(res)
```