### 滑动窗口
如果我们依次递增地枚举子串起始的位置，那么**子串的位置也是递增的**，假设当前从字符串的第$k$个位置作为子串的起始位置，得到了不包含重复字符的最长子串的位置为$r_k$。当我们选择$k+1$作为新的起始位置，可以保证的是从$k+1$到$r_k$的字符仍然是不重复的，我们可以继续增大$r_k$，直到右侧出现了重复的字符。

使用**滑动窗口**解决这个问题：
- 使用两个指针表示字符串的某个子串的左右边界，其中左指针代表**枚举子串的起始位置**；
- 每一步操作中先**判断右指针字符是否重复**，若重复则**依次向后枚举直到哈希表中不再出现该右指针字符**，并记录当前最长字符串长度。
- 直到右指针枚举到字符串末尾。

**代码**
*C++ version*
```
class Solution{
public:
    int lengthOfLongestSubstring(string s){
        unordered_set<char> sliding_window;
        int left = 0, right = 0;
        int max_str_len = 0;
        while(right < s.size()){
            while(sliding_window.find(s[right]) != sliding_window.end()){
                sliding_window.erase(s[left++]);
            }
            sliding_window.insert(s[right]);
            max_str_len = max(max_str_len, right - left + 1);
            right;
        }
    }
    return max_str_len;
};
```
*python version*
```
class Solution:
    def lengthOfLongestSubstring(self, s: str)->int:
        occ = set()
        left = max_sub_len = 0
        for right in range(len(s)):
            while s[right] in occ:
                occ.remove(s[left])
                left += 1
            occ.add(s[right])
            max_sub_len = max(max_sub_len, right - left + 1)
        return max_sub_len
```