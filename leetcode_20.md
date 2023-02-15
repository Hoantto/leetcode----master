### 模拟栈
经典的括号匹配问题
**代码**
*C++ version*
```
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char,char> brackets = {
            {')','('},
            {'}','{'},
            {']','['}
        };
        stack<char> process;
        for(char c:s){
            if(brackets.find(c) == brackets.end()){
                process.push(c);
            }else{
                if(!process.empty() && process.top() == brackets[c]){
                    process.pop();
                }else{
                    return false;
                }
            }
        }
        if(process.empty()) return true;
        else return false;
    }
};
```
*python version*
```
class Solution:
    def __init__(self,):
        self.brackets = {
            ')':'(',
            '}':'{',
            ']':'['
        }
        self.process = []
    def isValid(self, s: str) -> bool:
        for ch in s:
            if self.brackets.get(ch) is None:
                self.process.append(ch)
            else:
                length = len(self.process)
                if length and self.process[length - 1] is self.brackets[ch]:
                    self.process.pop()
                else:
                    return False
        if len(self.process):
            return False
        else:
            return True

```

**参考**
- [C++ map api](https://cplusplus.com/reference/map/map/)
- [python list api](https://docs.python.org/3/tutorial/datastructures.html)
- [python dict api](https://www.w3schools.com/python/python_ref_dictionary.asp)