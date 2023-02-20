### 字典序问题 + 双指针
熟悉字典序原理以及`C++ sort`和`python iterable.sort和sorted(iterable)`的`api`即可。其中`sorted(iterable)`可以接上返回值用于部分排序。

**代码**

*C++ version*
```
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int len = nums.size();
        auto swap = [&](int i, int j){
            int tmp;
            tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        };
        if(len == 1) return;
        int left = len - 1;
        while(left > 0){
            if(nums[left - 1] < nums[left]){
                int right = len - 1;
                while(right >= left){
                    if(nums[right] > nums[left - 1]){
                        swap(right, left - 1);
                        break;
                    }else{
                        right--;
                    }
                }
                sort(nums.begin() + left, nums.end());
                return;
            }else{
                left--;
            }
        }
        if(left == 0){
           sort(nums.begin(), nums.end());
           return;
        }
    }
};
```

*python version*
```
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        length = len(nums)
        if length == 1:
            return;
        left = length - 1
        def swap(x, y):
            tmp = nums[x]
            nums[x] = nums[y]
            nums[y] = tmp
        while left > 0:
            if nums[left - 1] < nums[left]:
                right = length - 1
                while right >= left:
                    if nums[right] > nums[left - 1]:
                        swap(right, left- 1)
                        nums[left:] = sorted(nums[left:])
                        return
                    else:
                        right -= 1
            else:
                left -= 1
        if left == 0:
            nums.sort()
            return
```

**参考**
- [关于python：将列表的一部分排序到位](https://www.codenong.com/2272819/)
- [C++ vector api](https://cplusplus.com/reference/vector/vector/)
- [组合学，从n到m无重复的排列产生器](https://zh.planetcalc.com/4242/)
- [字典序和下一个排列](https://blog.csdn.net/qq_33594380/article/details/82377923)