### 双指针
同`leetcode_26`。

**代码**
*C++ version*
```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;
        for(int j = 0; j < nums.size(); j++){
            if(nums[j] == val){
                continue;
            }else{
                nums[i++] = nums[j];
            }
        }
        return i;
    }
};
```

*python version*
```
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        for j in range(len(nums)):
            if nums[j] is val:
                continue
            else:
                nums[i] = nums[j]
                i += 1
        return i
```