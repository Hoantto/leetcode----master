### 回溯 + 剪枝
与上一题一致，均为`N皇后`问题。

**代码**
*C++ version*
```
class Solution {
public:
    vector<bool> col;
    vector<bool> rLean;
    vector<bool> lLean;
    int ret = 0;

    void dfs(int n, int pos){
        if(pos == n){
            ret++;
            return;
        }
        for(int i = 0; i < n; i++){
            if(col[i] | rLean[pos + i] | lLean[pos - i + n - 1]){
                continue;
            }
            col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = true;
            dfs(n, pos + 1);
            col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = false;
        }
        return;
    }

    int totalNQueens(int n) {
        col.resize(n, false);
        rLean.resize(2 * n - 1, false);
        lLean.resize(2 * n - 1, false);
        dfs(n , 0);
        return ret;
    }
};
```

*python version*
```
class Solution:
    def totalNQueens(self, n: int) -> int:
        col = [False for _ in range(n)]
        rLean = [False for _ in range(2 * n - 1)]
        lLean = [False for _ in range(2 * n - 1)]
        ret = 0
        def dfs(pos):
            nonlocal ret
            if pos == n:
                ret += 1
                return
            for i in range(n):
                if col[i] | rLean[pos + i] | lLean[pos - i + n - 1]:
                    continue
                col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = True
                dfs(pos + 1)
                col[i] = rLean[pos + i] = lLean[pos - i + n - 1] = False
            return
        dfs(0)
        return ret
```