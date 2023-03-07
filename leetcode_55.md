### 贪心算法
在遍历数组的过程中，维护**当前可以达到的最远下标**，当且仅当当前下标在可以到达的范围内便更新`max_pos = max(max_pos, i + nums[i])`，遍历过程中如果出现**当前可以到达的最远下标**结果大于数组长度的情况则返回`true`，当遍历完数组均未出现该情况则返回`false`。

**代码**
*C++ version*
```
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int most_far_pos = 0;
        for(int i = 0; i < n; i++){
            if(i <= most_far_pos){
                most_far_pos = max(most_far_pos, i + nums[i]);
                if(most_far_pos >= n - 1){
                    return true;
                }
            }
        }
        return false;
    }
};
```

*python version*
```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        n, most_far_pos = len(nums), 0
        for i in range(n):
            if i <= most_far_pos:
                most_far_pos = max(most_far_pos, i + nums[i])
                if most_far_pos >= n - 1:
                    return True 
        return False

```