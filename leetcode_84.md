### 单调栈
首先归纳一下枚举[高]的方法：
- 首先我们枚举一根柱子$i$作为高$h=heights[i]$
- 随后我们需要进行向左右两边扩展，使得扩展到的柱子的高度均不小于$h$。换句话说，我们需要找到**左右两侧最近的高度小于$h$的柱子**，这样这两根柱子之间（不包括其本身）的所有柱子高度均不小于$h$，并且就是$i$能够扩展到的最远范围。

那么我们先来看看如何求出**一根柱子的左侧且最近的小于其高度的柱子**。
> 对于两根柱子$j_0$以及$j_1$，如果$j_0<j_1$并且$heights[j_0]\ge heights[j_1]$，那么**对于任意的在它们之后出现的柱子$i(j_1 < i)$，$j_0$一定不是&i&左侧且最近的小于其高度的柱子**。

换句话说，我们可以对数组从左向右进行遍历，同时维护一个[可能作为答案]的数据结构，其中按照从小到大的顺序存放了一些$j$值。根据上面的结论，如果我们存放了$j_0,j_1,...,j_s$，那么一定有$heights[j_0]<heights[j_1]<...<heights[j_s]$。因为如果有两个相邻的$j$值对应的高度不满足$<$关系，那么后者会[挡住]前者，前者就不可能作为答案了。

这样我们在枚举到第$i$根柱子的时候，就可以先把所有高度大于等于$heights[i]$的$j$值全部移除。剩下的$j$值中高度最高的即为答案。在这之后，我们将$i$放入数据结构中，开始接下来的枚举。
- 栈中存放了$j$值。从栈底到栈顶，$j$的值严格单调递增，同时对应的高度值也严格单调递增
- 当我们枚举到第$i$根柱子时，我们从栈顶不断地移除$heights[j]\ge heights[i]$的$j$值。在移除完毕后，栈顶的$j$值就一定满足$heights[j]<heights[i]$，此时$j$就是$i$左侧且最近的小于其高度的柱子。
    - 这里会有一种特殊的情况，如果我们移除了栈中所有的$j$值，那么就说明$i$左侧所有的柱子的高度都大于$heights[i]$，那么我们可以认为$i$左侧且最近的小于其高度的柱子在位置$j=-1$，它是一根[虚拟]的、高度无限低的柱子。也称这根[虚拟]的柱子为[哨兵]
- 我们再将$i$放入栈顶

在这种方法中在对位置$i$进行入栈操作时，确定了它的左边界，从直觉上来说，与之对应的我们在对位置$i$进行出栈操作时可以确定它的右边界。当位置$i$被弹出栈时，说明此时遍历到的位置$i_0$的高度**小于等于$heights[i]$**，并且在$i_0$与$i$之间没有其他高度小于等于$heights[i]$的柱子。这是因为，如果在$i$和$i_0$之间还有其他位置的高度小于等于$heights[i]$的，那么在遍历到那个位置的时候，$i$应该已经被弹出栈了。所以位置$i_0$就是位置$i$的右边界。

**代码**
*C++ version*
```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n), right(n, n);
        stack<int> mono_stack;
        for(int i = 0; i < n; i++){
            while(!mono_stack.empty() && heights[mono_stack.top()] >= heights[i]){
                right[mono_stack.top()] = i;
                mono_stack.pop();
            }
            left[i] = (mono_stack.empty()? -1 : mono_stack.top());
            mono_stack.push(i);
        }
        int ans = 0;
        for(int i = 0; i < n; i++){
            ans = max(ans, (right[i] - left[i] - 1) * heights[i]);
        }
        return ans;
    }
};
```

*python version*
```
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        left = [-1 for _ in range(n)]
        right = [n for _ in range(n)]
        mono_stack = []
        for i in range(n):
            while len(mono_stack) != 0 and heights[mono_stack[-1]] >= heights[i]:
                right[mono_stack[-1]] = i
                mono_stack.pop()
            left[i] = -1 if len(mono_stack) == 0 else mono_stack[-1]
            mono_stack += [i]
        ans = 0
        for i in range(n):
            ans = max(ans, (right[i] - left[i] - 1) * heights[i])
        return ans

```

**参考**
- [Python List/Array Methods](https://www.w3schools.com/python/python_ref_list.asp)
- [std::stack](https://cplusplus.com/reference/stack/stack/)
