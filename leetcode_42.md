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
