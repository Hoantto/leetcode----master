### 排序
如果按照区间的左端点排序，那么在排序完成后的列表中，可以合并的区间一定是连续的。
![graph1](./1.png)
需要注意的是：在`python`中`sort(key lambda)`中的`lamda`公式只需要显示指明需要比较的`object`即可，不需要给出`binary comp`的结果

**代码**
*C++ version*
```
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [&](auto a,auto b){
            return a[0] < b[0];
        });
        vector<vector<int>> output;
        int left = intervals[0][0];
        int right = intervals[0][1];
        for(int i = 0; i < intervals.size(); i++){
            if(intervals[i][0] <= right){
                right = max(intervals[i][1], right);
            }else{
                vector<int> tmp = {left, right};
                output.push_back(tmp);
                left = intervals[i][0];
                right = intervals[i][1];
            }
            if(i == intervals.size() - 1){
                vector<int> tmp = {left, right};
                output.push_back(tmp);
            }
        }
        return output;
    }
};
```

*python version*
```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        output = []
        intervals.sort(key=lambda x: x[0])
        left = intervals[0][0]
        right = intervals[0][1]
        for i in range(len(intervals)):
            if intervals[i][0] <= right:
                right = max(right, intervals[i][1])
            else:
                output += [[left, right]]
                left = intervals[i][0]
                right = intervals[i][1]
            if i == len(intervals) - 1:
                output += [[left, right]]
        return output
```
**参考**
- [TypeError: <lambda>() missing 1 required positional argument](https://www.reddit.com/r/learnpython/comments/q6j7h8/typeerror_lambda_missing_1_required_positional/)
- [Custom Python list sorting](https://stackoverflow.com/questions/11850425/custom-python-list-sorting)