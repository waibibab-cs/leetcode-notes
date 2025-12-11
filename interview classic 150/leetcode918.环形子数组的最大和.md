# 918、环形子数组的最大和
mid

cpp
## 题解
一种情况是没有跨边界，直接使用53.最大子数组和nums_max即可

另一种情况是跨了边界，这时候实际上等价于数组元素总和减去最小子数组和nums_sum - nums_min

取上面两种情况的最大值即可

然而，还有一种更加特殊的情况是，数组所有元素为负值，这时候nums_sum - nums_min == 0，这时候再取max(nums_max, nums_sum - nums_min)显然错误，此时的答案应该是nums_max

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int nums_max = INT_MIN;
        int nums_sum = 0;
        int nums_min = INT_MAX;
        int count = 0;
        //先获取最大子数组和与最小子数组和
        for (int i = 0; i < nums.size(); i ++ ) {
            nums_sum += nums[i];
            count += nums[i];
            nums_max = max(count, nums_max);
            if (count < 0) count = 0;
        }
        for (int i = 0; i < nums.size(); i ++ ) {
            count += nums[i];
            nums_min = min(count, nums_min);
            if (count > 0) count = 0;
        }
        if (nums_sum - nums_min == 0) return nums_max; 
        else return max(nums_sum - nums_min, nums_max);
    }
};
```
