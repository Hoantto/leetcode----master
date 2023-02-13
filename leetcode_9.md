### itos的实现方法
[C++实现方法](https://stackoverflow.com/questions/5590381/easiest-way-to-convert-int-to-string-in-c)
[python实现方法](https://www.geeksforgeeks.org/convert-integer-to-string-in-python/)

#### stoi的实现方法
- [C++实现方法](https://www.freecodecamp.org/news/string-to-int-in-c-how-to-convert-a-string-to-an-integer-example/#:~:text=One%20effective%20way%20to%20convert,the%20integer%20version%20of%20it.)
- [Python实现方法](https://www.freecodecamp.org/news/python-convert-string-to-int-how-to-cast-a-string-in-python/#:~:text=To%20convert%2C%20or%20cast%2C%20a,int(%22str%22)%20.)

**代码**
*C++ version*
```
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;
        string s = to_string(x);
        int i = 0, j = s.size() - 1;
        while(s[i] == s[j] && i < j){
            i++;
            j--;
        }
        if(s.size() % 2){
            if(i == j) return true;
        }else{
            if(i - j == 1) return true;
        }
        return false;
    }
};
```
*python version*
```
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0: return False
        s = str(x)
        i = 0
        j = len(s) - 1
        while s[i] == s[j] and i < j:
            i += 1
            j -= 1
        if len(s) % 2:
            if i == j: return True
        else:
            if i - 1 == j: return True
        return False

```

**复杂度分析**
> - 时间复杂度：$O(n)$。
> - 空间复杂度：$O(n)$。