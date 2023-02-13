### 模拟
**若存在小的数字在大的数字的左边的情况**，根据规则需要减去小的数字。对于这种情况，我们也可以将每个字符视作一个单独的值，若一个数字右侧的数字比它大，则将该数字的符号取反。

**代码**
*C++ version*
```
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> roman = {
            {'I', 1},
            {'V', 5},
            {'X', 10},
            {'L', 50},
            {'C', 100},
            {'D', 500},
            {'M', 1000},
        };
        int ans = 0;
        int n = s.size();
        for(int i = 0; i < n; i++){
            int value = roman[s[i]];
            if(i < n -1 && value < roman[s[i + 1]]){
                ans -= value;
            }else{
                ans += value;
            }
        }
        return ans;
    }
};
```
*python version*
```
class Solution:
    def romanToInt(self, s: str) -> int:
        roman = {
            'I': 1,
            'V': 5,
            'X': 10,
            'L': 50,
            'C': 100,
            'D': 500,
            'M': 1000, 
        }
        ans = 0
        n = len(s)
        for i, ch in enumerate(s):
            value = roman[ch]
            if i < n - 1 and value < roman[s[i + 1]]:
                ans -= value
            else:
                ans += value
        return ans
```

**复杂度分析**
> - 时间复杂度：$O(n)$。
> - 空间复杂度：$O(1)$。
