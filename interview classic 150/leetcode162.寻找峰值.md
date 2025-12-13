# 162、寻找峰值
cpp；mid
## 题解
二分

首先这道题：nums[-1]=-∞、nums[n]=-∞、nums[i] != nums[i+1]就意味着一定存在峰值

设左右指针分别为l，r

其代表的含义是：[0,l]之间一定没有峰值，[r,n-1]之间有峰值，当l + 1 == r时，峰值就是r

l、r如何收敛？当nums[mid]<nums[mid+1]，峰值必在mid+1及其右边，则l=mid，反之，峰值必在mid及左边，r=mid

如何初始化？l=-1,r=n-1；为什么？

对于常规的二分问题而言，实际上应该初始化为:l=-1,r=n，因为对于mid=(l+r)/2而言，mid最小就是lr分别取最小，l=-1，r=1，此时mid最小为0，mid最大就是lr分别取最大，l=n-2，r=n，mid最大的就是n-1，此时mid取值为[0,n-1]恰好落在区间内
但是这道题要判断峰值，涉及mid+1，因此如果mid取值还是[0,n-1]就会右出界，因此，我们让r初始化为n-1，这时，mid取值为[0,n-2]满足情况

但是这样初始化会不会漏了峰值唯一且索引为n-1的情况？不会的，因为r一开始就是n-1，如果峰值就是n-1，r会始终指向n-1，最终返回也是n-1

C++
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        if (nums.size() == 1) return 0;
        int l = -1, r = nums.size() - 1;
        while (l + 1 != r) {
            int mid = (l + r) >> 1;
            if (nums[mid] < nums[mid + 1]) l = mid;
            else r = mid;
        }
        return r;
    }
};
```
