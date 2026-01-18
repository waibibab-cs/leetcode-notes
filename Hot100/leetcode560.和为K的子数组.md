# leetcode560.和为k的子数组
本题要求时间复杂度O(n)

如果直接暴力，为$`O(n^3)`$

通过引入前缀和，可以优化为$`O(n^2)`$，我们观察此复杂度下的源码：

C++
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int res = 0;
        int n = nums.size();
        vector<int> S(n + 1, 0);
        unordered_map<int, int> cnt;
        for (int i = 1; i <= n; i ++ ) S[i] = S[i - 1] + nums[i - 1];
        for (int r = 1; r <= n; r ++ ) {
            for (int l = 0; l < r; l ++ ) {
                if (S[r] - S[l] == k) res ++ ;
            }
        }
        return res;
    }
};
```

发现其中：
```cpp
for (int l = 0; l < r; l ++ ) {
    if (S[r] - S[l] == k) res ++ ;
}
```
这段代码的含义为：给定一个端点r，需要找到区间0——r-1中有几个l满足：`S[l] == S[r] - k`，因此我们可以设置一个哈希表，快速统计区间0~r-1各前缀和出现的次数，比如当遍历到r时，cnt[s[r]-k]就代表在区间 [0, r-1] 范围内，前缀和等于 S[r]-k 的左端点 l 的个数，因此代码可以优化为

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int res = 0;
        unordered_map<int, int> cnt;
        cnt[0] = 1;
        vector<int> S(nums.size() + 1, 0);
        for (int i = 1; i <= nums.size(); i ++ ) S[i] = S[i - 1] + nums[i - 1];
        for (int r = 1; r <= nums.size(); r ++ ) {
            res += cnt[S[r] - k];
            cnt[S[r]] ++ ;
        }
        return res;
    }
};
```

另：为什么先更新res再更新cnt？因为如果反过来的话，cnt[s[r]-k]就代表的区间就是 [0, r]而不是[0,r-1]了
