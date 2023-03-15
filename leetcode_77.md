### 组合
使用`itertools.combinations api`即可。

**代码**
*python version*
```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        output = []
        for tmp in itertools.combinations(range(1, n + 1), k):
            output += [tmp]
        return output
```
#### 递归method
注意剪枝方法。

**代码**
*python version*
```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        output = []
        tmp = []
        def dfs(cur):
            nonlocal tmp, output
            # tmp长度加上区间[cur, n]的长度小于k，不可能构造出长度为k的tmp
            if len(tmp) + (n - cur + 1) < k:
                return
            if len(tmp) == k:
                output += [copy.deepcopy(tmp)]
                return
            tmp += [cur]
            dfs(cur + 1)
            tmp.pop()
            dfs(cur + 1)
            return
        dfs(1)
        return output
```

**参考**
- [itertools — Functions creating iterators for efficient looping](https://docs.python.org/3/library/itertools.html)