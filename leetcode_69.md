### 二分查找
需要注意的是`C++`需要注意数据字长的限制。

**代码**
*C++ version*
```
class Solution {
public:
    int mySqrt(int x) {
        long long left = 1, right = x;
        while(left <= right){
            long long mid = (left + right) / 2;
            if(mid * mid > x){
                right = mid - 1;
            }else if(mid * mid < x){
                left = mid + 1;
            }else{
                return mid;
            }
        }
        return (long long)(left + right)/2;
    }
};
```

*python version*
```
class Solution:
    def mySqrt(self, x: int) -> int:
        left, right = 1, x
        while left <= right:
            mid = (left + right) // 2
            if mid * mid > x:
                right = mid - 1
            elif mid * mid < x:
                left = mid + 1
            else:
                return mid
        return (left + right)//2
```