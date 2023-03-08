### 排序
与`leetcode_56`一致，插入排序后做区间合并即可。

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        intervals.push_back(newInterval);
        sort(intervals.begin(), intervals.end(), [&](auto a, auto b){
            return a < b;
        });
        vector<vector<int>> output;
        for(auto interval:intervals){
            int left = interval[0];
            int right = interval[1];
            if(output.empty() || left > output.back()[1]){
                output.push_back({left, right});
            }else{
                output.back()[1] = max(output.back()[1], right);
            }
        }
        return output;
    }
};
```

*python version*
```
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        intervals += [newInterval]
        intervals.sort(key = lambda x: x[0])
        output = []
        for left, right in intervals:
            if len(output) == 0 or left > output[-1][1]:
                output += [[left, right]]
            else:
                output[-1][1] = max(output[-1][1], right)
        return output
```