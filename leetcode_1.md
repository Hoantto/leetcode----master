### 哈希表
本题的主要任务是快速地找出`target - x`,时间复杂度较高的原因是寻找`target - x`的时间复杂度过高。

使用**哈希表**，可以将寻找`target - x`的复杂度从$O(N)$降低到$O(1)$。

这样我们创建一个哈希表，对于每一个`x`，我们首先查询哈希表中是否存在`target - x`，然后将`x`插入到哈希表中，**可以保证不会让`x`和自己匹配**。

**代码**

*C++ version*
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target){
        /*do not need ranked*/
        unordered_map<int, int> value_loc;
        vector<int> res(2, -1);
        for(int i = 0; i < nums.size(); i++){
            value_loc.insert(pair<int, int>(nums[i], i));
        }
        for(int i = 0; i < nums.size(); i++){
            if(value_loc.count(target - nums[i]) > 0 && value_loc[target - nums[i]] != i){
                res[0] = i;
                res[1] = value_loc[target - nums[i]];
                break;
            }
        }
        return res;
    }
};
```
*python version*
```
class Solution:
    def twoSum(self, nums:List[int], target:int) -> List[int]:
        hashtable = dict()
        for i, num in enumerate(nums):
            if target - num in hashtable:
                return [hashtable[target - num] , i]
            hashtable[num] = i
        return []
```

**复杂度分析**
> - 时间复杂度：$O(N)$，其中`N`是数组中元素的数量。对于每一个元素`x`，我们可以$O(1)$地寻找`target - x`。
> - 空间复杂度：$O(N)$，其中`N`是数组中的元素数量。主要为哈希表的开销。