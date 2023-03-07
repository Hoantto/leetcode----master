### 动态规划 or 贪心算法

#### 动态规划method
利用`f[i]`表示**以下标i结尾的最大连续子数组和**，则状态转移方程为：
$$f[i]=max{f[i-1]+nums[i],nums[i]}$$
边界条件为`f[i]=nums[0]`，*note*:可以利用滚动数组使得空间复杂度为$O(N)$

**代码**
*C++ version*
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int pre = 0, max_sub = nums[0];
        for(auto x: nums){
            pre = max(pre + x, x);
            max_sub = max(max_sub, pre);
        }
        return max_sub;
    }
};
```
*python version*
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        pre, max_sub = 0, nums[0]
        for x in nums:
            pre = max(pre + x, x)
            max_sub = max(max_sub, pre)
        return max_sub
```

#### 贪心算法method
贪心点在于每次计算下标为`i`结束位置的连续序列和后，若`sum<0`则抛弃前面的所有和，记`sum=num[i]`。

**代码**
*C++ version*
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int cur_total = nums[0];
        int max_sub = nums[0];
        for(int i = 1; i < nums.size(); i++){
            cur_total = cur_total > 0? cur_total: 0;
            cur_total += nums[i];
            max_sub = max(max_sub, cur_total);
        }
        return max_sub;
    }
};
```
*python version*
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        cur_total, max_sub = nums[0], nums[0]
        for i in range(1, len(nums)):
            cur_total = cur_total if cur_total > 0 else 0
            cur_total += nums[i]
            max_sub = max(max_sub, cur_total)
        return max_sub
```