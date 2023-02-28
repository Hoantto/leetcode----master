### 负数标记哈希表
利用负数来实现对数组的标记，原来`for(auto& c:nums)`能直接用`c = xxxx`来实现赋值啊，代码离屎山越来越远了。

**代码**
*C++ version*
```
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for(int& num: nums){
            if(num <= 0){
                num = n + 1;
            }
        }
        for(int i = 0; i < n; i++){
            int num = abs(nums[i]);
            if(num <= n){
                nums[num - 1] = -abs(nums[num - 1]);
            }
        }
        for(int i = 0; i < n; i++){
            if(nums[i] > 0){
                return i + 1;
            }
        }
        return n + 1;
    }
};
```

*python version*
```
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
        for i, num in enumerate(nums):
            if num <= 0:
                nums[i] = n + 1
        for i, num in enumerate(nums):
            num = abs(num)
            if num <= n:
                nums[num - 1] = -abs(nums[num - 1])
        for i in range(n):
            if nums[i] > 0:
                return i + 1
        return n + 1
```