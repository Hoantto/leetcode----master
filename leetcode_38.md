### 字符串
1. Create a helper function that maps an integer to pairs of its digits and their frequencies. For example, if you call this function with "223314444411", then it maps it to an array of pairs [[2,2], [3,2], [1,1], [4,5], [1, 2]].

2. Create another helper function that takes the array of pairs and creates a new integer. For example, if you call this function with [[2,2], [3,2], [1,1], [4,5], [1, 2]], it should create "22"+"23"+"11"+"54"+"21" = "2223115421".

3. Now, with the two helper functions, you can start with "1" and call the two functions alternatively n-1 times. The answer is the last integer you will obtain.

**代码**
*C++ version*
```
class Solution {
public:
    vector<pair<char, int>> seq2Pair(string& s){
        vector<pair<char, int>> res;
        int num = 1;
        char c = s[0];
        if(s.size() == 1){
            res.push_back(pair<char, int>(c, num));
            return res;
        }
        for(int i = 1; i < s.size(); i++){
            if(s[i] != s[i - 1]){
                res.push_back(pair<char, int>(c, num));
                c = s[i];
                num = 1;
            }else{
                num++;
            }
            if(i == s.size() - 1){
                    res.push_back(pair<char, int>(c, num));
                }
        }
        return res;
    }

    string pair2Seq(vector<pair<char,int>>& res){
        string output = "";
        for(auto p:res){
            char c = p.first;
            int num = p.second;
            string n = to_string(num);
            output += n;
            output += c;
        }
        return output;
    }
    string countAndSay(int n) {
        string init = "1";
        while(--n){
            vector<pair<char,int>> res = seq2Pair(init);
            init = pair2Seq(res);
        }
        return init;
    }
};
```

*python version*
```
class Solution:
    def countAndSay(self, n: int) -> str:
        def seq2Pair(s):
            res = []
            length = len(s)
            c = s[0]
            num = 1
            if length == 1:
                res.append([c, num])
            for i in range(1, length):
                if s[i] != s[i - 1]:
                    res.append([c, num])
                    c = s[i]
                    num = 1
                else:
                    num += 1
                if i == length - 1:
                    res.append([c, num])
            return res
        def pair2Seq(res):
            output = []
            for c, num in res:
                n = str(num)
                output.append(n)
                output.append(c)
            ret = "".join(output)
            return ret
        init = "1"
        n = n - 1
        while(n):
            n -= 1
            res = seq2Pair(init)
            init = pair2Seq(res)
        return init
```

**参考**
- [C++ to_string api](https://cplusplus.com/reference/string/to_string/)