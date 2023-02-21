### 二分查找
将数组一分为二，其中**一定有一个是有序的**，另一个可能是有序的，也就是部分有序。此时有序的部分可以用二分查找。无序的部分再一分为二，其中一个一定是有序的，另一个可能有序，可能无序，就这样循环寻找即可。

**代码**
*C++ version*
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int len = nums.size();
        if(!len) return -1;
        if(len == 1){
            return nums[0] == target? 0: -1;
        }
        int left = 0, right = len - 1;
        while(left <= right){
            int mid = (left + right) / 2;
            if(nums[mid] == target) return mid;
            if(nums[0] <= nums[mid]){
                //有序
                if(nums[0] <= target && target < nums[mid]){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }else{
                //无序
                if(nums[mid] < target && target <= nums[len - 1]){
                    left = mid + 1;
                }else{
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
};
```

*python version*
```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        length = len(nums)
        if length == 0:
            return -1
        if length == 1:
            return 0 if nums[0] == target else -1
        left, right = 0, length- 1
        while left <= right:
            mid = (int)((left + right) / 2)
            if nums[mid] == target:
                return mid
            if nums[left] <= nums[mid]:
                if nums[left] <= target and target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target and target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return -1
```