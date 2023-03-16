### 双指针
本题要求相同元素最多出现两次而非一次，使用需要检查上上一个应该被保留的元素$num[slow-2]$是否和当前待检查元素$nums[fast]$相同。而不是和$num[slow-1]$对比。

**代码**
*C++ version*
```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n <= 2) return n;
        int slow = 2, fast = 2;
        while(fast < n){
            if(nums[slow - 2] != nums[fast]){
                nums[slow] = nums[fast];
                slow++;
            }
            fast++;
        }
        return slow;
    }
};
```

*python version*
```
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        n = len(nums)
        if n <= 2:
            return n
        slow, fast = 2, 2
        while fast < n:
            if nums[slow - 2] != nums[fast]:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```