### 递归+数学
- 当我们要计算$x^n$时，我们可以先递归地计算出$y=x^{\lfloor n/2 \rfloor}$，其中$\lfloor a \rfloor$表示对$a$进行下取整
- 根据递归的结果，如果$n$为偶数，那么$x^n=y^2$；如果$n$为奇数，那么$x^n=y^2 \times x$
- 递归的边界为$n = 0$，任意数的0次方均为1

**代码**
*C++ version*
```
class Solution {
public:
    double quickMul(double x, long long N){
        if(N == 0){
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0? y * y: y * y * x;
    }
    double myPow(double x, int n) {
        long long N = n;
        return N >= 0? quickMul(x, N): 1.0 / quickMul(x, -N);
    }
};
```

*python version*
```
class Solution:
    def myPow(self, x: float, n: int) -> float:
        def quickMul(x, N):
            if N == 0:
                return 1.0
            y = quickMul(x, N // 2)
            return y * y * x if N % 2 else y * y
        return quickMul(x, n) if n >= 0 else 1.0 / quickMul(x, -n)
```