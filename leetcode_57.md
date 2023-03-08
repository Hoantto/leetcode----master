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

### 遍历合并
**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int left = newInterval[0];
        int right = newInterval[1];
        vector<vector<int>> output;
        bool placed = false;
        for(auto interval:intervals){
            if(interval[0] > right){
                if(!placed){
                    output.push_back({left, right});
                    placed = true;
                }
                output.push_back(interval);
            }
            else if(interval[1] < left){
                output.push_back(interval);
            }
            else{
                left = min(left, interval[0]);
                right = max(right, interval[1]);
            }
        }
        if(!placed){
            output.push_back({left, right});
        }
        return output;
    }
};
```

*python version*
```
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        left, right = newInterval[0], newInterval[1]
        output = []
        placed = False
        for l, r in intervals:
            if r < left:
                output += [[l, r]]
            elif l > right:
                if placed == False:
                    output += [[left, right]]
                    placed = True
                output += [[l, r]]
            else:
                left = min(left, l)
                right = max(right, r)
        if placed == False:
            output += [[left, right]]
        return output
```