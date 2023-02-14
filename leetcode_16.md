### 排序+双指针
**类似leetcode_15**
**代码**
*C++ version*
```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int n = nums.size();
        int clo_value = 1e5;
        auto update = [&](int res){
            if(abs(target - res) < abs(target - clo_value)){
                clo_value = res;
            }
        };
        sort(nums.begin(), nums.end());
        for(int first = 0; first < n; first++){
            if(first > 0 && nums[first] == nums[first - 1]) continue;
            int second = first + 1;
            int third = n - 1;
            while(second < third){
                if(nums[first] + nums[second] + nums[third] == target) return target;
                update(nums[first] + nums[second] + nums[third]);
                if(nums[first] + nums[second] + nums[third] > target){
                    int new_third = third - 1;
                    while(second < new_third && nums[third] == nums[new_third]){
                        new_third--;
                    }
                    third = new_third;
                }else{
                    int new_second = second + 1;
                    while(new_second < third && nums[second] == nums[new_second]){
                        new_second++;
                    }
                    second = new_second;
                }
            }
        }
        return clo_value;
    }
};
```

*python version*
```
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort()
        clo_value = 1e5
        update = lambda res: res if abs(target - res) < abs(target - clo_value) else clo_value
        n = len(nums)
        for left in range(n):
            if left > 0 and nums[left] == nums[left - 1]:
                continue
            second = left + 1
            third = n - 1
            while second < third:
                if nums[left] + nums[second] + nums[third] == target:
                    return target
                clo_value = update(nums[left] + nums[second] + nums[third])
                if nums[left] + nums[second] + nums[third] > target:
                    new_third = third - 1
                    while second < new_third and nums[third] == nums[new_third]:
                        new_third -= 1
                    third = new_third
                else:
                    new_second = second + 1
                    while new_second < third and nums[second] == nums[new_second]:
                        new_second += 1
                    second = new_second
        return clo_value
                    
```
**参考**
- (python lambda)[https://realpython.com/python-lambda/]