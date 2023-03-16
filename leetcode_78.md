### 回溯
组合类的回溯题，需要用增序来去重。

**代码**
*C++ version*
```
class Solution {
public:
    vector<int> tmp;
    vector<vector<int>> output;
    void dfs(vector<int>& nums, int pos){
        if(pos == nums.size()){
            output.push_back(tmp);
            return;
        }
        dfs(nums, pos + 1);
        int num = nums[pos];
        tmp.push_back(num);
        dfs(nums, pos + 1);
        tmp.pop_back();
        return;
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        dfs(nums, 0);
        return output;
    }
};
```

*python version*
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        tmp = []
        output = []
        def dfs(pos):
            nonlocal output, tmp
            if pos == len(nums):
                output += [tmp[:]]
                return
            dfs(pos + 1)
            num = nums[pos]
            tmp += [num]
            dfs(pos + 1)
            tmp.pop()
            return
        dfs(0)
        return output
```