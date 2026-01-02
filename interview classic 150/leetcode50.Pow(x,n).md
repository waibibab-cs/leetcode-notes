# 50、Pow（x，n）
注意使用llabs

C++
```cpp
class Solution {
public: 
    double myPow(double x, int n) {
        if (n < 0) return myPow(1 / x, llabs(n));
        double res = 1;
        while (n) {
            if (n & 1) res *= x;
            x *= x;
            n >>= 1;   
        }
        return res;
    }
};
```
