# 4、寻找两个正序数组的中位数
hard；cpp
## 题解
最简单的思路是将两个数组元素进行有序合并，然后直接得到中位数，但是这种方法的时间复杂度不满足题意

实际上对于两个数组A和B，长度分别为a和b，则中位数就是第(a + b) / 2 + 1个数（当(a+b)为奇数时，偶数时应为第(a+b)/2个数和第(a+b)/2+1个数的平均值），所以任务就转换为求A和B的第K个数

整体思路是这样的：
* 先求A的第k/2个数a1，再求B的第k - k/2个数b1
* 比较a1和b1：
  * 如果a1<b1，将a1及之前的x个元素从A中移除掉，再求A和B的第k-x个数
  * 如果a1>b1，将b1及之前的y个元素从B中移除掉，再求A和B的第k-y个数
 
C++
```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        double res;
        int n = nums1.size() + nums2.size();
        if (n % 2 == 1) {
            res = find(nums1, 0, nums2, 0, n / 2 + 1);
        } else 
        {
            double a = find(nums1, 0, nums2, 0, n / 2);
            double b = find(nums1, 0, nums2, 0, n / 2 + 1);
            res = (a + b) / 2.0;
        }
        return res;
    }
    int find(vector<int>& nums1, int i, vector<int>& nums2, int j, int k) {
        // 保证nums1剩余元素的个数大于nums2剩余元素的个数
        if (nums1.size() - i > nums2.size() - j) return find(nums2, j, nums1, i, k);
        if (i == nums1.size()) return nums2[j + k - 1];
        if (k == 1) return min(nums1[i], nums2[j]);
        int si = min((int)nums1.size() - 1, i + k / 2 - 1);
        int sj = j + k - k / 2 - 1;
        if (nums1[si] < nums2[sj]) {
            return find(nums1, si + 1, nums2, j, k - (si - i + 1));
        } else 
        {
            return find(nums1, i, nums2, sj + 1, k - (sj - j + 1));
        }
    }
};
```
