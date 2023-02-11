### 二分查找
给定两个**有序**的数组，找到两个数组的中位数有一下的两种思路：
- 使用归并的方法，合并两个有序的数组，得到一个较大的数组。得合并后的中位数即可。
- 找到中位数的位置。由于两个数组的长度已知。维护两个指针，初始时分别指向两个数组的下标0的位置，每次将指向较小值的指针后移一位。如果一个指针已经达到数组的末尾，只需移动另一个数组的指针，直到达到中位数的位置。

假设两个有序数组分别为$A$和$B$。要找到第$k$个元素，可以比较$A[k/2-1]$和$B[k/2-1]$，由于$A[k/2-1]$和$B[k/2-1]$前面分别有$A[ 0..k/2-2]$和$B[ 0..k/2-2]$，即有$k/2-1$个元素，对于$A[ k/2-1]$和$B[ k/2-1]$中的较小值，最多只会有$(k/2-1)\times 2\le k-2$个元素比它们小，它们不会是第$k$小的数。

可以分为以下三种情况：
> - 如果$A[k/2-1]<B[k/2-1]$，比$A[k/2-1]$小的数最多有$k-2$个，$A[0]$到$A[k/2-1]$也都不可能是第$k$个数，可以全部排除。
> - 如果$A[k/2-1]>B[k/2-1]$，可以排除$B[0]$到$B[k/2-1]$。
> - 如果$A[k/2-1]=B[k/2-1]$，可以归入第一种情况处理。

**有三种情况需要特殊处理**
> - 如果$A[k/2-1]$或者$B[k/2-1]$越界，可以直接选取对应数组的最后一个元素，在这种情况下必须**根据排除的个数减少k的值**。
> - 如果一个数组为空，说明该数组中的所有元素都被排除，我们可以直接返回另一个数组中第$k$小的元素。
> - 如果$k=1$，只需要返回两个数组的首元素的最小值即可。

**代码**
*C++ version*
```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    int len = nums1.size() + nums2.size();
    if(len % 2 == 1){
        return getKthElement(nums1, nums2, (len + 1) / 2);
    }else{
        return (getKthElement(nums1, nums2, len / 2) + getKthElement(nums1, nums2, len / 2 + 1)) / 2.0;
    }
}
int getKthElement(const vector<int>& nums1, const vector<int>& nums2, int k){
    int m = nums1.size(), n = nums2.size();
    int index1 = 0, index2 = 0;
    while(true){
        if(index1 == m){
            return nums2[index2 + k - 1];
        }
        if(index2 == n){
            return nums1[index1 + k - 1];
        }
        if(k == 1){
            return min(nums1[index1], nums2[index2]);
        }
        int newIndex1 = min(index1 + k/2 - 1, m - 1);
        int newIndex2 = min(index2 + k/2 - 1, n - 1);
        int pivot1 = nums1[newIndex1];
        int pivot2 = nums2[newIndex2];
        if(pivot1 <= pivot2){
            k -= newIndex1 - index1 + 1;
            index1 = newIndex1 + 1;
        }else{
            k -= newIndex2 - index2 + 1;
            index2 = newIndex2 + 1;
        }
    }
}
};
```
*python version*
```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        def getKthElement(k):
            nums1_len, nums2_len = len(nums1), len(nums2)
            index1 = index2 = 0
            while True:
                if index1 == nums1_len:
                    return nums2[index2 + k - 1]
                if index2 == nums2_len:
                    return nums1[index1 + k - 1]
                if k == 1:
                    return min(nums1[index1], nums2[index2])
                new_index1 = min(index1 + k//2 - 1, nums1_len - 1)
                new_index2 = min(index2 + k//2 - 1, nums2_len - 1)
                pivot1, pivot2 = nums1[new_index1], nums2[new_index2]
                if pivot1 <= pivot2:
                    k -= new_index1 - index1 + 1
                    index1 = new_index1 + 1
                else:
                    k -= new_index2 - index2 + 1
                    index2 = new_index2 + 1
        length = len(nums1) + len(nums2)
        if length % 2:
            return getKthElement((length + 1)//2)
        else:
            return (getKthElement(length//2) + getKthElement(length//2 + 1))/2
            
```
