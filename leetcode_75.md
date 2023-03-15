### 双指针
经典的**三色国旗**问题，主要是掌握不用语言中的`swap`技巧。

**代码**
*C++ version*
```
class Solution {
public:
    void sortColors(vector<int>& nums) {
    int size = nums.size();
    int p0 = 0, p1 = 0;
    for(int i = 0; i < size; i++){
        if(nums[i] == 1){
            swap(nums[i], nums[p1++]);
        }else if(nums[i] == 0){
            swap(nums[i], nums[p0]);
            if(p0 < p1){
                swap(nums[i], nums[p1]);
            }
            p0++, p1++;
        }
    }
}
};
```

*python version*
```
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        p0, p1 = 0, 0
        for i in range(n):
            if nums[i] == 1:
                nums[i], nums[p1] = nums[p1], nums[i]
                p1 += 1
            elif nums[i] == 0:
                nums[i], nums[p0] = nums[p0], nums[i]
                if p0 < p1:
                    nums[i], nums[p1] = nums[p1], nums[i]
                p0 += 1
                p1 += 1

```