### 哈希 + 回溯
本题在求出组合的过程中就进行去重的操作，可以考虑将**相同的数放在一起进行处理**，也就是说，如果$x$出现了$y$次，那么在递归时一次性地处理它们，分别调用选择$0,1,...,y$次$x$的递归函数，这样我们就不会得到重复的组合，需要使用哈希表得到元素和元素个数之间的映射。在`python`中可以使用`collections.Counter(iterable)`来快速构造哈希表，再使用`items`转化为`list`。

**代码**
*C++ version*
```
class Solution {
public:
    vector<pair<int, int>> freq;
    vector<vector<int>> output;
    vector<int> tmp;
    void dfs(int pos, int res){
        if(res == 0){
            output.push_back(tmp);
            return;
        }
        if(pos == freq.size() || res < freq[pos].first){
            return;
        }
        int most = min(res / freq[pos].first, freq[pos].second);
        for(int i = 1; i <= most; i++){
            tmp.push_back(freq[pos].first);
            dfs(pos + 1, res - i * freq[pos].first);
        }
        for(int i = 1; i <= most; i++){
            tmp.pop_back();
        }
        dfs(pos + 1, res);
        return;
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        for(auto num: candidates){
            if(freq.empty() || num != freq.back().first){
                freq.emplace_back(num, 1);
            }else{
                ++freq.back().second;
            }
        }
        dfs(0, target);
        return output;
    }
};
```

*python version*
```
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(pos, res):
            nonlocal tmp
            if res == 0:
                output.append(tmp[:])
                return
            if pos == len(freq) or res < freq[pos][0]:
                return
            dfs(pos + 1, res)
            most = min(res // freq[pos][0], freq[pos][1])
            for i in range(1, most + 1):
                tmp.append(freq[pos][0])
                dfs(pos + 1, res - i * freq[pos][0])
            tmp = tmp[:-most]
            return
        freq = sorted(collections.Counter(candidates).items())
        output = []
        tmp = []
        dfs(0, target)
        return output

```