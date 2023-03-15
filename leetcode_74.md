### 二分查找
只需要完成线性到二维数据下标的映射即可。

**代码**
*C++ version*
```
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        int left = 0, right = m * n - 1;
        while(left <= right){
            int mid = (left + right) / 2;
            int i = mid / n;
            int j = mid % n;
            int num = matrix[i][j];
            if(num == target) return true;
            else if(num < target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return false;
    }
};
```

*python version*
```
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        left, right = 0, m * n - 1
        while left <= right:
            mid = (left + right) // 2
            i = mid // n
            j = mid % n
            num = matrix[i][j]
            if num == target:
                return True
            elif num < target:
                left = mid + 1
            else:
                right = mid - 1
        return False
```