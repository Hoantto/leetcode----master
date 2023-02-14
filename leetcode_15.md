### 排序+双指针
题目要求找所有**不重复**且和为0的三元组，因为**不重复**所以无法简单使用三重循环枚举所有的三元组。
**不重复**只需要保证：
- 第二重循环枚举到的元素不小于当前第一重循环枚举到的元素；
- 第三重循环枚举到的元素不小于当前第二重循环枚举到的元素。
也就是说，枚举的三元组$(a,b,c)$满足$a \le b \le c$，这样就减少了重复。
同时，对于每一重循环而言，相邻两次枚举的元素不能相同，否则也会造成重复。
如果固定了前两重循环枚举到的元素$a$和$b$，那么只有唯一的$c$满足$a+b+c=0$。当第二重循环往后枚举一个元素$b^{\prime}$时，由于$b^{\prime}>b$，那么满足$a+b^{\prime}+c^{\prime}=0$的$c^{\prime}$一定有$c^{\prime}<c$，也即$c^{\prime}$在数组中一定出现在$c$的左侧。就是说可以从小到大枚举$b$，**同时**从大到小枚举$c$，即**第二重循环和第三重循环实际上是并列的关系**。
就可以保持第二重循环不变，而将**第三重循环变成一个从数组最右端开始向左移动的指针**。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        for(int first = 0; first < n; first++){
            //需要和上一次枚举的数不同
            if(first > 0 && nums[first] == nums[first - 1]) continue;
            int third = n - 1;
            int target = -nums[first];
            for(int second = first + 1; second < n; second++){
                //需要和上一次枚举的数不同
                if(second > first + 1 && nums[second] == nums[second - 1]) continue;
                //保证second在third的左侧
                while(second < third && nums[second] + nums[third] > target){
                    --third;
                }
                if(second == third) break;
                if(nums[second] + nums[third] == target){
                    ans.push_back({nums[first], nums[second], nums[third]});
                }
            }
        }
        return ans;
    }
};
```
*python version*
```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        nums.sort()
        ans = []
        for first in range(n):
            #需要和上一次枚举的数不相同
            if first > 0 and nums[first] == nums[first - 1]:
                continue
            third = n - 1
            target = -nums[first]
            for second in range(first + 1, n):
                #需要和上一次枚举的数不相同
                if second > first + 1 and nums[second] == nums[second - 1]:
                    continue
                while second < third and nums[second] + nums[third] > target:
                    third -= 1
                if second == third:
                    break
                if nums[second] + nums[third] == target:
                    ans.append([nums[first], nums[second], nums[third]])
        return ans
```
**参考**
- [python sort](https://www.freecodecamp.org/news/python-sort-how-to-sort-a-list-in-python/)
- [C++ sort](https://cplusplus.com/reference/algorithm/sort/)

**复杂度分析**
> - 时间复杂度：$O(N^2)$。
> - 空间复杂度：$O(logN)$，是额外排序的空间复杂度。