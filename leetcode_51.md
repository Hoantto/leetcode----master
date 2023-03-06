### 回溯 + 剪枝
经典的N皇后问题，主要是对同一列`col[i]`、同一右斜线`rLean[pos + i]`、同一左斜线`lLean[pos - i + n - 1]`的剪枝。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<string>> output;
    vector<string> tmp;
    vector<bool> col;
    vector<bool> rLean;
    vector<bool> lLean;

    void dfs(int n, int pos){
        if(pos == n){
            output.push_back(tmp);
            return;
        }
        for(int i = 0; i < n; i++){
            if(col[i] | rLean[pos + i] | lLean[pos - i + n - 1]){
                continue;
            }
            col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = true;
            tmp[pos][i] = 'Q';
            dfs(n, pos + 1);
            tmp[pos][i] = '.';
            col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = false;
        }
        return;
    }

    vector<vector<string>> solveNQueens(int n) {
        tmp.resize(n, string(n, '.'));
        col.resize(n ,false);
        rLean.resize(2 * n - 1, false);
        lLean.resize(2 * n - 1, false);
        dfs(n, 0);
        return output;

    }
};
```

*python version*
```
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        output = []
        tmp = [["." for _ in range(n)] for _ in range(n)]
        col = [False for _ in range(n)]
        rLean = [False for _ in range(2 * n - 1)]
        lLean = [False for _ in range(2 * n - 1)]

        def dfs(pos):
            nonlocal output
            if pos == n:
                output += [copy.deepcopy(tmp)]
                return
            for i in range(n):
                if col[i] | rLean[pos + i] | lLean[pos - i + n - 1]:
                    continue
                col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = True
                tmp[pos][i] = "Q"
                dfs(pos + 1)
                tmp[pos][i] = "."
                col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = False
            return
        dfs(0)
        ret = []
        for chess in output:
            chess_str = ["".join(line) for line in chess]
            ret += [chess_str]
        return ret
```