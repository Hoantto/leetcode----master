### 回溯 + 去重
排列问题其实也是一样的套路。
`定长tmp回溯和变长tmp回溯的区别也需要注意`
****
**还要强调的是去重一定要对元素进行排序，这样我们才方便通过相邻的节点来判断是否重复使用了**。

**代码**
*C++ version*
```
class Solution {
public:
    vector<int> used;

    void dfs(vector<int>& nums, vector<vector<int>>& output, int idx, vector<int>& tmp){
        if(idx == nums.size()){
            output.push_back(tmp);
            return;
        }
        for(int i = 0; i < nums.size(); i++){
            // 去重
            if(used[i] || (i > 0 && nums[i] == nums[i - 1] && !used[i - 1])){
                continue;
            }
            tmp.push_back(nums[i]);
            used[i] = 1;
            dfs(nums, output, idx + 1, tmp);
            used[i] = 0;
            tmp.pop_back();
        }
        return;
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> output;
        vector<int> tmp;
        used.resize(nums.size());
        sort(nums.begin(), nums.end());
        dfs(nums, output, 0, tmp);
        return output;
    }
};
```

*python version*
```
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        output = []
        tmp = []
        used = [False for _ in range(len(nums))]
        def dfs(pos, tmp):
            if pos == len(nums):
                output.append(tmp[:])
                return
            for i in range(len(nums)):
                if used[i] or (i > 0 and nums[i] == nums[i - 1] and used[i - 1] == False):
                    continue
                tmp.append(nums[i])
                used[i] = True  
                dfs(pos + 1, tmp)
                tmp.pop()
                used[i] = False
            return
        nums.sort()
        dfs(0, tmp)
        return output
```