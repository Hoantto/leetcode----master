### 排序+双指针
与`三数之和`不同的是，可以进行一些剪枝的操作：
- 在确定第一个数之后，如果$nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target$，说明此时剩下的三个数无论取什么值，四数之和一定大于$target$，因此退出第一重循环。
- 在确定第一个数之后，如果$nums[i]+nums[n-3]+nums[n-2]+nums[n-1]<target$，说明此时剩下的三个数无论取什么值，四数之和一定小于$target$，因此第一重循环直接进入下一轮，枚举$nums[i+1]$。
- 在确定前两个数之后，如果$nums[i]+nums[j]+nums[j+1]+nums[j+2]>target$，说明此时剩下的两个数无论取什么值，匹配之和一定大于$target$，因此退出第二重循环。
- 在确定前两个数之后，如果$nums[i]+nums[j]+nums[n-2]+nums[n-1]<target$，说明此时剩下的两个数无论取什么值，四数之和一定小于$target$，因此第二重循环直接进入下一轮，枚举$nums[j+1]$。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res;
        if(nums.size() < 4){
            return res;
        }
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for(int first = 0; first < n - 3; first++){
            if(first > 0 && nums[first] == nums[first - 1]) continue;
            //优化
            if((long)nums[first] + nums[first + 1] + nums[first + 2] + nums[first + 3] > target) break;
            if((long)nums[first] + nums[n - 1] + nums[n - 2] + nums[n - 3] < target) continue;
            for(int second = first + 1; second < n - 2; second++){
                if(second > first + 1 && nums[second] == nums[second - 1]) continue;
                //优化
                if((long)nums[first] + nums[second] + nums[second + 1] + nums[second + 2] > target) break;
                if((long)nums[first] + nums[second] + nums[n - 1] + nums[n - 2] < target) continue;
                int left = second + 1, right = n - 1;
                while(left < right){
                    long sum = (long)nums[first] + nums[second] + nums[left] + nums[right];
                    if(sum == target){
                        res.push_back({nums[first], nums[second], nums[left], nums[right]});
                        while(left < right && nums[left] == nums[left + 1]){
                            left++;
                        }
                        left++;
                        while(left < right && nums[right] == nums[right - 1]){
                            right--;
                        }
                        right--;
                    }
                    if(sum < target){
                        int new_left = left + 1;
                        while(new_left < right && nums[new_left] == nums[left]){
                            new_left++;
                        }
                        left = new_left;
                    }
                    if(sum > target){
                        int new_right = right - 1;
                        while(new_right > left && nums[new_right] == nums[right]){
                            new_right--;
                        }
                        right = new_right;
                    }
                }
            }
        }
        return res;
    }
};
```
*python version*
```
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        quadruplets = list()
        if not nums or len(nums) < 4:
            return quadruplets
        
        nums.sort()
        length = len(nums)
        for i in range(length - 3):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            if nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target:
                break
            if nums[i] + nums[length - 3] + nums[length - 2] + nums[length - 1] < target:
                continue
            for j in range(i + 1, length - 2):
                if j > i + 1 and nums[j] == nums[j - 1]:
                    continue
                if nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target:
                    break
                if nums[i] + nums[j] + nums[length - 2] + nums[length - 1] < target:
                    continue
                left, right = j + 1, length - 1
                while left < right:
                    total = nums[i] + nums[j] + nums[left] + nums[right]
                    if total == target:
                        quadruplets.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left + 1]:
                            left += 1
                        left += 1
                        while left < right and nums[right] == nums[right - 1]:
                            right -= 1
                        right -= 1
                    elif total < target:
                        left += 1
                    else:
                        right -= 1
        
        return quadruplets


```
