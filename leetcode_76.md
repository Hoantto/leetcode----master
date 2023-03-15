### 滑动窗口 + 哈希表
只需要遍历字符串时，维护滑动窗口，记录每一时刻的窗口起始位置和窗口的长度即可。

**代码**
*C++ version*
```
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> need, window;
        for(auto c:t){
            need[c]++;
        }
        int left = 0, right = 0;
        int valid = 0;
        int ans_left = 0, length = s.size() + 1;
        while(right < s.size()){
            char c = s[right++];
            if(need.count(c)){
                window[c]++;
                if(need[c] == window[c]){
                    valid++;
                }
            }
            while(valid == need.size()){
                if(right - left < length){
                    length = right - left;
                    ans_left = left;
                }
                char o = s[left++];
                if(need.count(o)){
                    if(need[o] == window[o]){
                        --valid;
                    }
                    --window[o];
                }
            }
        }
        return length == s.size() + 1? "":s.substr(ans_left, length);
    }
};
```

*python version*
```
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        need = {}
        window = {}
        for c in t:
            if need.get(c) == None:
                need[c] = 1
            need[c] += 1
        left, right = 0, 0
        valid = 0
        ans_left = 0
        length = len(s) + 1
        while right < len(s):
            ch = s[right]
            right += 1
            if need.get(ch) != None:
                if window.get(ch) == None:
                    window[ch] = 1
                window[ch] += 1
                if window[ch] == need[ch]:
                    valid += 1
            while valid == len(need):
                if right - left < length:
                    length = right - left
                    ans_left = left
                o = s[left]
                left += 1
                if need.get(o) != None:
                    if need[o] == window[o]:
                        valid -= 1
                    window[o] -= 1
        return "" if length == len(s) + 1 else s[ans_left:ans_left+length]

```

**参考**
- [Python Dictionary Methods](https://www.w3schools.com/python/python_ref_dictionary.asp)
- [std::string::substr](https://cplusplus.com/reference/string/string/substr/)
- [std::map](https://cplusplus.com/reference/map/map/)