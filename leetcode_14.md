### 遍历
**代码**
*C++ version*
```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(!strs.size()){
            return "";
        }
        int size = strs[0].size();
        int len = strs.size();
        for(int i = 0; i < size; i++){
            char ch = strs[0][i];
            for(int j = 1; j < len; j++){
                if(i >= strs[j].size() || strs[j][i] != ch){
                    return strs[0].substr(0, i);
                }
            }
        }
        return strs[0];
    }
};
```

*python version*
```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not len(strs):
            return ""
        for num, ch in enumerate(strs[0]):
            for s in strs[1:]:
                if num >= len(s) or ch != s[num]:
                    return strs[0][:num]
        return strs[0]
```

**参考**
- [python sliding](https://stackoverflow.com/questions/509211/understanding-slicing)