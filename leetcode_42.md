### 接雨水问题
#### 动态规划方法
下标为$i$处的接雨水的量为$...,i-1$处的最高位置与$i+1,...$处的最高位置中的最小值和$height[i]$决定的。$water[i]=max(0, height[i] - max(leftMax,rightMax))$。类似木桶原理，$heifht[i]$为地面的高度。

**代码**
*C++ version*
```
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        vector<int> left(n), right(n);
        for(int i = 1; i < n; i++){
            left[i] = max(left[i - 1], height[i - 1]);
        }
        for(int i = n - 2; i >= 0; i--){
            right[i] = max(right[i + 1], height[i + 1]);
        }
        int water = 0;
        for(int i = 0; i < n; i++){
            int level = min(left[i], right[i]);
            water += max(0, level - height[i]);
        }
        return water;
    }
};

```
*python version*
```
class Solution:
    def trap(self, height: List[int]) -> int:
        n = len(height)
        left = [0 for _ in range(n)]
        right = [0 for _ in range(n)]
        for i in range(1, n):
            left[i] = max(left[i - 1], height[i - 1])
            if i < n:
                right[-1 - i] = max(right[-i], height[-i])
        water = 0
        for i in range(n):
            level = min(left[i], right[i])
            water += max(0, level - height[i])
        return water
```

#### 单调栈
1. 单调递减栈
 - 当后面的柱子比前面低时，是无法接住雨水的
 - 当找到一根比前面高的柱子，就可以计算接到的雨水
 - 所以需要使用**单调递减栈**
2. 对更低的柱子进行入栈操作
 - 更低的柱子以为这后面如果能找到高的柱子，这里就可以接到雨水，所以入栈把它保存起来
 - 平地相当于高度为0的柱子
3. 当出现高于栈顶的柱子时
 - 说明可以对前面的柱子进行结算了
 - 计算已经接到的雨水，然后出栈前面更低的柱子

**代码**
*C++ version*
```
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        stack<int> st;
        for(int i = 0; i < height.size(); i++){
            while(!st.empty() && height[st.top()] < height[i]){
                int cur = st.top();
                st.pop();
                if(st.empty()) break;
                int l = st.top();
                int r = i;
                int h = min(height[r], height[l]) - height[cur];
                ans += (r - l - 1) * h;
            }
            st.push(i);
        }
        return ans;
    }
};
```

*python version*
```
class Solution:
    def trap(self, height: List[int]) -> int:
        ans = 0
        stack = []
        for i in range(len(height)):
            while len(stack) != 0 and height[stack[-1]] < height[i]:
                cur = stack[-1]
                stack.pop()
                if len(stack) == 0:
                    break
                l = stack[-1]
                r = i
                h = min(height[l], height[r]) - height[cur]
                ans += (r - l - 1) * h
            stack.append(i)
        return ans
```

**参考**
 - [【接雨水】单调递减栈，简洁代码，动图模拟](https://leetcode.cn/problems/trapping-rain-water/solution/trapping-rain-water-by-ikaruga/)