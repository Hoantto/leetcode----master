### 双指针
假设数组$nums$的长度为$n$。将快指针$fast$依次遍历从$1$到$n-1$的每个位置，对于每个位置，如果$nums[fast] \ne nums[fast-1]$，说明$nums[fast]$和之前的元素都不同，因此将$nums[fast]$的值复制到$nums[slow]$，然后将$slow$的值加$1$，也即指向下一个位置。
遍历结束后，从$nums[0]$到$nums[slow-1]$的每一个元素都不相同且包含原数组中的每个不同的元素，因此新的长度为$slow$，返回$slow$即可。

**代码**
*C++ version*
```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i = 0;
        for(int j = 0; j < nums.size(); j++){
            if(j == 0 || nums[j] != nums[j - 1]){
                nums[i++] = nums[j];
            }else{
                continue;
            }
        }
        return i;
    }
};
```
*python version*
```
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0
        
        n = len(nums)
        fast = slow = 1
        while fast < n:
            if nums[fast] != nums[fast - 1]:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        
        return slow

```