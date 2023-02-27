### 回溯
由于本题可以重复选择，只需要在当前决策层选择：
1. 放入当前位置的数据，继续尝试放入当前位置数据
2. 不放入当前位置的数据，进入下一决策层
最后利用$res < 0$来进行剪枝即可。

**代码**
*C++ version*
```
class Solution {
public:
    vector<int> tmp;
    vector<vector<int>> output;
    void dfs(vector<int> candidates, int pos, int res){
        if(pos == candidates.size()){
            return;
        }
        if(res == 0){
            output.push_back(tmp);
            return;
        }
        if(res < 0){
            return;
        }
        int num = candidates[pos];
        tmp.push_back(num);
        dfs(candidates, pos, res - num);
        tmp.pop_back();
        dfs(candidates, pos + 1, res);
        return;
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        dfs(candidates, 0, target);
        return output;
    }
};
```

*python version*
```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        output = []
        tmp = []
        def dfs(pos, res):
            nonlocal output
            if pos == len(candidates) or res < 0:
                return
            if res == 0:
                output.append(copy.deepcopy(tmp))
                return
            num = candidates[pos]
            tmp.append(num)
            dfs(pos, res - num)
            tmp.pop()
            dfs(pos + 1, res)
            return
        dfs(0, target)
        return output
```

**参考**
- [python List method](https://www.programiz.com/python-programming/methods/list)
- [python copy.deepcopy](https://www.geeksforgeeks.org/copy-python-deep-copy-shallow-copy/)