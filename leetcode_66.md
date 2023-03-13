### 数组模拟

**代码**
*C++ version*
```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        vector<int> output;
        int a, b;
        int carry = 0;
        for(int i = digits.size() - 1; i >= 0; i--){
            if(i == digits.size() - 1){
                a = (digits[i] + 1) / 10;
                b = (digits[i] + 1) % 10;
                if(a){
                    carry = 1;
                }else{
                    carry = 0;
                }
                output.push_back(b);
            }else{
                a = (digits[i] + carry) / 10;
                b = (digits[i] + carry) % 10;
                if(a){
                    carry = 1;
                }else{
                    carry = 0;
                }
                output.push_back(b);
            }
        }
        if(carry){
            output.push_back(1);
        }
        reverse(output.begin(), output.end());
        return output;
    }
};
```

*python version*
```
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        output = []
        carry = 0
        first = False
        for num in digits[::-1]:
            if first == False:
                first = True
                a = (num + 1) // 10
                b = (num + 1) % 10
                if(a):
                    carry = 1
                else:
                    carry = 0
                output += [b]
            else:
                a = (num + carry) // 10
                b = (num + carry) % 10
                if(a):
                    carry = 1
                else:
                    carry = 0
                output += [b]
        if carry:
            output += [1]
        output.reverse()
        return output
```

**参考**
- [Python List/Array Methods](https://www.w3schools.com/python/python_ref_list.asp)