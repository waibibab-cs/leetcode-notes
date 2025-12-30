# 201、数字范围按位与
mid

cpp
## 题解
我们观察0-15的二进制数：

0000 0001 0010 0011
0100 0101 0110 0111
1000 1001 1010 1011
1100 1101 1110 1111

对于第1位（从右到左数第二个位），呈现2个0、2个1的循环

对于第2位（从右到左数第三个位），呈现4个0、4个1的循环

总结得到这个循环周期为2的x的次方，x为位数

那么可以推出：如果left和right在第x位均为1，且right-left < pow(2,x)，说明[left,right]之间的所有数的第x位必为1，证明如下：

反证法，假设[left,right]之间存在某数第x位为0，我们让(right-left)取最小值，发现最小值为pow(2,x) + 1，不满足right-left < pow(2,x)，因此假设不成立

cpp
```cpp
class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        int res = 0;
        for (int i = 0; i < 32; i ++ ) {
            int a = left >> i & 1;
            int b = right >> i & 1;
            if (a && b && (right - left) <= (1 << i)) res += (1 << i);
        }
        return res;
    }
};
```
