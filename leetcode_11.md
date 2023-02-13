### 双指针+贪心
从数组的**两个边界**开始向中间遍历，每次淘汰较小者，希望找到跟大的高。

**代码**
*C++ version*
```
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0, right = height.size() - 1;
        int max_area = 0;
        while(left < right){
            max_area = max(max_area, min(height[left], height[right]) * (right - left));
            height[left] < height[right]?left++:right--;
        }
        return max_area;
    }
};
```
*python version*
```
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left = max_area = 0
        right = len(height) - 1
        while left < right:
            max_area = max(max_area, min(height[left],height[right])*(right-left))
            if height[left] < height[right]:
                left += 1
            else: right -= 1
        return max_area
```
**参考**
[python中的三元表达](https://www.cnblogs.com/mywood/p/7416893.html)

**复杂度分析**
> - 时间复杂度：$O(n)$。
> - 空间复杂度：$O(1)$。
