### 回溯
回溯的过程中维护一个字符串，表示已有的字母排列(如果没有遍历完电话号码中所有的数字，则已有的字母排列是不完整的)。**回溯算法用于寻找所有可能的解**，如果发现一个解不可行，则会舍弃不可行的解。

**代码**
*C++ version*
```
class Solution {
public:
    string tmp;
    vector<string> res;
    vector<string> board={"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
    void DFS(int pos, string digits){
        if(pos == digits.size()){
            res.push_back(tmp);
            return;
        }
        int num = digits[pos] - '0';
        for(int i = 0; i < board[num].size(); i++){
            tmp.push_back(board[num][i]);
            DFS(pos + 1, digits);
            tmp.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        if(digits.empty()){
            return res;
        }
        DFS(0, digits);
        return res;
    }
};
```
*python version*
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return list()
        phoneMap = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz",
        }

        def backtrack(index: int):
            if index == len(digits):
                combinations.append("".join(combination))
            else:
                digit = digits[index]
                for letter in phoneMap[digit]:
                    combination.append(letter)
                    backtrack(index + 1)
                    combination.pop()
        
        combination = list()
        combinations = list()
        backtrack(0)
        return combinations
```
**技巧**
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return list()
        phoneMap = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz",
        }

        groups = [phoneMap[digit] for digit in digits]
        return ["".join(combination) for combination in itertools.product(*groups)]
```

**参考**
- [python itertools.product\permutations](https://docs.python.org/3/library/itertools.html)

