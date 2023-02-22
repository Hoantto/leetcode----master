### 哈希表 + 模拟
本题的关键在于：**如何求得当前下标[i, j]的`box`的值**，其中$box\_idx=(i/3)\times3+(j/3)$。

**代码**
*C++ version*
```
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<vector<int>> row (9, vector<int>(9,0));
        vector<vector<int>> col (9, vector<int>(9,0));
        vector<vector<int>> box (9, vector<int>(9,0));

        for(int i=0;i<9;i++){
            for(int j=0;j<9;j++){
                if(board[i][j] == '.'){
                    continue;
                }
                int val = board[i][j] - '1';
                int box_index = (i/3) * 3 + j/3;
                if(row[i][val] == 0 && col[j][val] == 0 && box[box_index][val] == 0){
                    row[i][val] = 1;
                    col[j][val] = 1;
                    box[box_index][val] = 1;
                }
                else{
                    return false;
                }
            }
        }
        return true;
    }
};
```

*python version*
```
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        raw = [[0 for _ in range(9)] for _ in range(9)]
        col = [[0 for _ in range(9)] for _ in range(9)]
        box = [[0 for _ in range(9)] for _ in range(9)]
        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    continue
                else:
                    idx = int((i//3)*3 +(j//3))
                    c = (int)(ord(board[i][j]) - ord('1'))
                    if raw[i][c] == 0 and col[j][c] == 0 and box[idx][c] == 0:
                        raw[i][c] = 1
                        col[j][c] = 1
                        box[idx][c] = 1
                    else:
                        return False
        return True
```

**参考**
- [python // operator](https://www.w3schools.com/python/python_operators.asp)
- [python How to get the ASCII value of a character](https://stackoverflow.com/questions/227459/how-to-get-the-ascii-value-of-a-character)
- [C++ set api](https://cplusplus.com/reference/set/set/)