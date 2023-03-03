### 回溯
最简单的回溯，需要加入使用标记数组`used`即可，主要需要掌握`python itertools.permutations api`的使用，返回值是生成的`tuples`。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<int>> output;
    vector<int> tmp;
    void dfs(vector<int>& nums, vector<bool>& used, int len){
        if(len == nums.size()){
            output.push_back(tmp);
            return;
        }
        for(int i = 0; i < used.size(); i++){
            if(used[i] == false){
                tmp.push_back(nums[i]);
                used[i] = true;
                dfs(nums, used, len + 1);
                tmp.pop_back();
                used[i] = false;
            }
        }
        return;
    }
    vector<vector<int>> permute(vector<int>& nums) {
        int n = nums.size();
        vector<bool> used(n, false);
        dfs(nums, used, 0);
        return output;
    }
};
```

*python version*
```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        output = []
        for permutation in itertools.permutations(nums):
            output += [permutation]
        return output
```

**参考**
- [itertools.permutations](https://docs.python.org/3/library/itertools.html#itertools.permutations)