### 哈希表+贪心
**代码**
*C++ version*
```
class Solution {
public:
    string intToRoman(int num) {
        map<int, string> roman = {
            {1,  "I"},
            {4,  "IV"},
            {5,  "V"},
            {9,  "IX"},
            {10, "X"},
            {40, "XL"},
            {50, "L"},
            {90, "XC"},
            {100, "C"},
            {400, "CD"},
            {500, "D"},
            {900, "CM"},
            {1000, "M"}
        };
        string res;
        while(num > 0){
            for(auto it = roman.rbegin(); it != roman.rend(); it++){
                if(num - it->first >= 0){
                    res += it->second;
                    num -= it->first;
                    break;
                }
            }
        }
        return res;
    }
};
```

*python version*
```
class Solution:

    def intToRoman(self, num: int) -> str:
        roman = [
        (1000, "M"),
        (900, "CM"),
        (500, "D"),
        (400, "CD"),
        (100, "C"),
        (90, "XC"),
        (50, "L"),
        (40, "XL"),
        (10, "X"),
        (9, "IX"),
        (5, "V"),
        (4, "IV"),
        (1, "I"),
        ]
        res = []
        while num > 0:
            for value, symbol in roman:
                if num - value >= 0:
                    res.append(symbol)
                    num -= value
                    break
        return "".join(res)
```

**参考**
- [C++ map api](https://cplusplus.com/reference/map/map/)
- [python list api](https://docs.python.org/3/tutorial/datastructures.html)
- [python str.join(iterable)](https://docs.python.org/3/library/stdtypes.html#str.join)

**复杂度分析**
> - 时间复杂度：$O(1)$。
> - 空间复杂度：$O(1)$。