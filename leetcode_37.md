### 回溯 + 位运算
回溯中需要注意的是**需要记录所有的空格的位置**来方便回溯。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<bool>> raw{9, vector<bool>(9, false)};
    vector<vector<bool>> col{9, vector<bool>(9, false)};
    vector<vector<bool>> box{9, vector<bool>(9, false)};
    vector<pair<int, int>> spaces;
    bool valid;
    void dfs(vector<vector<char>>& board, int pos){
        if(pos == spaces.size()){
            valid = true;
            return;
        }
        auto [i, j] = spaces[pos];
        for(int digit = 0; digit < 9 && !valid; digit++){
            int idx = (i / 3) * 3 + (j / 3);
            if(!raw[i][digit] && !col[j][digit] && !box[idx][digit]){
                raw[i][digit] = col[j][digit] = box[idx][digit] = true;
                board[i][j] = digit + '1';
                dfs(board, pos + 1);
                raw[i][digit] = col[j][digit] = box[idx][digit] = false;
            }
        }
    }
    void solveSudoku(vector<vector<char>>& board) {
        valid = false;
        for(int i = 0; i < 9; i++){
            for(int j = 0; j < 9; j++){
                if(board[i][j] == '.'){
                    spaces.push_back({i,j});
                }
                else{
                    int digit = board[i][j] - '1';
                    int idx = (i / 3) * 3 + (j / 3);
                    raw[i][digit] = col[j][digit] = box[idx][digit] = true;
                }
            }
        }
        dfs(board, 0);
    }
};
```

*python version*
```
class Solution:
    def __init__(self):
        self.valid = False
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        raw = [[False for _ in range(9)] for _ in range(9)]
        col = [[False for _ in range(9)] for _ in range(9)]
        box = [[False for _ in range(9)] for _ in range(9)]
        spaces = []

        def dfs(pos):
            if pos == len(spaces):
                self.valid = True
                return
            i, j = spaces[pos]
            for digit in range(9):
                if self.valid == True:
                    return
                idx_b = (int)((i // 3) * 3 + (j // 3))
                if raw[i][digit] == False and col[j][digit] == False and box[idx_b][digit] == False:
                    raw[i][digit] = True
                    col[j][digit] = True
                    box[idx_b][digit] = True
                    board[i][j] = str(digit + 1)
                    dfs(pos + 1)
                    raw[i][digit] = False
                    col[j][digit] = False
                    box[idx_b][digit] = False
                

        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    spaces.append([i, j])
                else:
                    digit = int(ord(board[i][j]) - ord('1'))
                    idx =(int)((i // 3) * 3 + (j // 3))
                    raw[i][digit] = True
                    col[j][digit] = True
                    box[idx][digit] = True
        dfs(0)
```

当$b$的二进制表示为$(011000100)_2$时，就表示数字$3,7,8$已经出现过。

**位运算的一些基础的使用技巧**：
- 对于第$i$行第$j$列的位置，$raw[i]\ |\ col[j]\ |\ box[idx]$中第$k$位为$1$，表示该位置不能填入数字$k+1$，如果对这个值进行$~$按位取反，那么第$k$位为$1$就表示该位置可以填入数字$k+1$，我们可以通过寻找$1$来进行枚举。由于在进行按位取反后，这个数的高位也全部变成了$1$，因此需要将这个数和$(111111111)_{2}=(1FF)_{16}$进行按位与运算&，将所有无关的位置为$0$。
- 可以利用异或运算$\land$，将第$i$位从$0$变为$1$，或从$1$变为$0$。与数$1<<i$进行按位异或运算即可。
- 可以用$b\&(-b)$得到$b$二进制表示中最低位的$1$。
- 可以利用$b\&(b-1)$去除最低位$1$。

**代码**
*C++ version*
```
class Solution {
private:
    int line[9];
    int column[9];
    int block[3][3];
    bool valid;
    vector<pair<int, int>> spaces;

public:
    void flip(int i, int j, int digit) {
        line[i] ^= (1 << digit);
        column[j] ^= (1 << digit);
        block[i / 3][j / 3] ^= (1 << digit);
    }

    void dfs(vector<vector<char>>& board, int pos) {
        if (pos == spaces.size()) {
            valid = true;
            return;
        }

        auto [i, j] = spaces[pos];
        int mask = ~(line[i] | column[j] | block[i / 3][j / 3]) & 0x1ff;
        for (; mask && !valid; mask &= (mask - 1)) {
            int digitMask = mask & (-mask);
            int digit = __builtin_ctz(digitMask);
            flip(i, j, digit);
            board[i][j] = digit + '0' + 1;
            dfs(board, pos + 1);
            flip(i, j, digit);
        }
    }

    void solveSudoku(vector<vector<char>>& board) {
        memset(line, 0, sizeof(line));
        memset(column, 0, sizeof(column));
        memset(block, 0, sizeof(block));
        valid = false;

        for (int i = 0; i < 9; ++i) {
            for (int j = 0; j < 9; ++j) {
                if (board[i][j] == '.') {
                    spaces.emplace_back(i, j);
                }
                else {
                    int digit = board[i][j] - '0' - 1;
                    flip(i, j, digit);
                }
            }
        }

        dfs(board, 0);
    }
};

```

*python version*
```
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        def flip(i: int, j: int, digit: int):
            line[i] ^= (1 << digit)
            column[j] ^= (1 << digit)
            block[i // 3][j // 3] ^= (1 << digit)

        def dfs(pos: int):
            nonlocal valid
            if pos == len(spaces):
                valid = True
                return
            
            i, j = spaces[pos]
            mask = ~(line[i] | column[j] | block[i // 3][j // 3]) & 0x1ff
            while mask:
                digitMask = mask & (-mask)
                digit = bin(digitMask).count("0") - 1
                flip(i, j, digit)
                board[i][j] = str(digit + 1)
                dfs(pos + 1)
                flip(i, j, digit)
                mask &= (mask - 1)
                if valid:
                    return
            
        line = [0] * 9
        column = [0] * 9
        block = [[0] * 3 for _ in range(3)]
        valid = False
        spaces = list()

        for i in range(9):
            for j in range(9):
                if board[i][j] == ".":
                    spaces.append((i, j))
                else:
                    digit = int(board[i][j]) - 1
                    flip(i, j, digit)

        dfs(0)


```

**参考**
- [Python bin()](https://www.programiz.com/python-programming/methods/built-in/bin)
- [Python nonlocal Keyword](https://www.w3schools.com/python/ref_keyword_nonlocal.asp)
- [C++ __builtin_ctz(x)](https://www.geeksforgeeks.org/builtin-functions-gcc-compiler/)
- [位运算](https://github.com/Mikoto10032/Interview-Notebook/blob/master/docs/notes/Leetcode%20%E9%A2%98%E8%A7%A3%20-%20%E4%BD%8D%E8%BF%90%E7%AE%97.md)
- [Vector declaration "expected parameter declarator"](https://stackoverflow.com/questions/39560277/vector-declaration-expected-parameter-declarator)
- [python How to get the ASCII value of a character](https://stackoverflow.com/questions/227459/how-to-get-the-ascii-value-of-a-character)