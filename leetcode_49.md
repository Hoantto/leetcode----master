### 哈希表
利用排序后字符串作为哈希表的`key`，可以严格区分。同样的也可以使用**同一类字符串的字符值相加相同**。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        for(auto& str: strs){
            auto key = str;
            sort(key.begin(), key.end());
            mp[key].push_back(str);
        }
        vector<vector<string>> output;
        for(auto it = mp.begin(); it != mp.end(); it++){
            output.push_back(it->second);
        }
        return output;
    }
};
```

*python version*
```
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        mp = {}
        for s in strs:
            key = "".join(sorted(s))
            if mp.get(key) == None:
                mp[key] = []
            mp[key] += [s]
        output = []
        for value in mp.values():
            output += [value]
        return output

```

**参考**
- [How to Handle Unhashable Type List Exceptions in Python](https://rollbar.com/blog/handling-unhashable-type-list-exceptions/)
- [How to insert new keys/values into Python dictionary?](https://www.tutorialspoint.com/How-to-insert-new-keys-values-into-Python-dictionary#:~:text=To%20insert%20new%20keys%2Fvalues%20into%20a%20Dictionary%2C%20use%20the,the%20new%20pair%20gets%20inserted.)
- [Python Dictionary Methods](https://www.w3schools.com/python/python_ref_dictionary.asp)