### 贪心
在遍历数组的过程中，当遍历到上一次能到达的最远距离`end`时，说明需要`step`步数增加。并且在到达上一次能到达的最远距离`end`的过程中，可以更新下一次能到达的最远距离`max_pos`。

使用**动态规划**的复杂度较高。
**代码**
*C++ version*
```
class Solution {
public:
    int jump(vector<int>& nums) {
        int max_pos = 0, n = nums.size();
        int end = 0, step = 0;
        for(int i = 0; i < n - 1; i++){
            if(max_pos >= i){
                max_pos = max(max_pos, i + nums[i]);
                // 到达上次跳跃到达的右边界了
                if(i == end){
                    end = max_pos;
                    step++;
                }
            }
        }
        return step;
    }
};
```

*python version*
```
class Solution:
    def jump(self, nums: List[int]) -> int:
        n, step = len(nums) - 1, 0
        max_pos, end = 0, 0
        for i in range(n):
            max_pos = max(max_pos, i + nums[i])
            if i == end:
                end = max_pos
                step += 1
        return step
```