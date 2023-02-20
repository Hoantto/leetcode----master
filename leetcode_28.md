### string api
**代码**
*C++ version*
```
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(haystack.find(needle) != string::npos){
            return haystack.find(needle);
        }else{
            return -1;
        }
    }
};
```

*python version*
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if haystack.find(needle) is not -1:
            return haystack.find(needle)
        else:
            return -1
```

**参考**
- [C++ string api](https://cplusplus.com/reference/string/string/)
- [python string method](https://www.w3schools.com/python/python_ref_string.asp)