### 二分查找
本题的关键在于在二分找到`nums[mid] == target`的目标`mid`值后，需要由`mid`向两边遍历扩展找到相同值的区间，扩展的边界为$left$和$right$。

**代码**
*C++ version*
```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int len = nums.size();
        vector<int> output(2, -1);
        if(!len || (len == 1 && nums[0] != target)) return output;
        int left = 0, right = len - 1;
        while(left <= right){
            int mid = (left + right) / 2;
            if(nums[mid] == target){
                int l1 = mid;
                int l2 = mid;
                while(l1 >= left){
                    if(nums[l1] == target){
                        l1--;
                    }else{
                        break;
                    }
                }
                while(l2 <= right){
                    if(nums[l2] == target){
                        l2++;
                    }else{
                        break;
                    }
                }
                output[0] = l1 + 1;
                output[1] = l2 - 1;
                return output;
            }
            else if(nums[mid] < target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return output;
    }
};
```

*python version*
```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        length = len(nums)
        if length == 0 or (length == 1 and nums[0] != target):
            return [-1,-1]
        left, right = 0, length - 1
        while(left <= right):
            mid = (int)((left + right) / 2)
            if nums[mid] == target:
                l1 = l2 = mid
                while l1 >= left:
                    if nums[l1] == target:
                        l1 -= 1
                    else:
                        break
                while l2 <= right:
                    if nums[l2] == target:
                        l2 += 1
                    else:
                        break
                return [l1 + 1, l2 - 1]
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return [-1, -1]
```
**复杂度分析**
- 时间复杂度：$O(logN)$
- 空间复杂度：$O(1)$