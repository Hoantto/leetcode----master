### 字符串模拟
本质上还是`大数相加`的解题思想。只是需要注意`C++ string::insert api`的重载中插入字符串和插入字符的函数参数不一样。

**代码**
*C++ version*
```
class Solution {
public:
    string addBinary(string a, string b) {
        int size_a = a.size();
        int size_b = b.size();
        int res = size_a - size_b;
        if(res < 0){
            res = abs(res);
            while(res--){
                a.insert(0, "0");
            }
        }else{
            res = abs(res);
            while(res--){
                b.insert(0, "0");
            }
        }
        int size = a.size();
        int carry = 0;
        string output = "";
        for(int i = size - 1; i >= 0; i--){
            int num_a = a[i] - '0';
            int num_b = b[i] - '0';
            int tmp = (num_a + num_b + carry) % 2;
            carry = (num_a + num_b + carry) / 2;
            output += ('0' + tmp);
        }
        if(carry){
            output += "1";
        }
        reverse(output.begin(), output.end());
        return output;
    }
};
```

*python version*
```
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        size_a, size_b = len(a), len(b)
        res = size_a - size_b
        if res < 0:
            res = abs(res)
            a = "".join(["0" for _ in range(res)]) + a
        else:
            res = abs(res)
            b = "".join(["0" for _ in range(res)]) + b
        carry = 0
        output = ""
        for num_a, num_b in zip(a[::-1], b[::-1]):
            num_a = int(num_a)
            num_b = int(num_b)
            tmp = (num_a + num_b + carry) % 2
            output = str(tmp) + output
            carry = (num_a + num_b + carry) // 2

        if carry:
            output = "1" + output
        return output
```

**参考**
- [std::string](https://cplusplus.com/reference/string/string/)