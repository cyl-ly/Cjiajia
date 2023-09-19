#### [整数拆分](https://leetcode.cn/problems/integer-break/)

> 给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。
>
> 返回 *你可以获得的最大乘积* 。

> ```
> 输入: n = 2
> 输出: 1
> 解释: 2 = 1 + 1, 1 × 1 = 1。
> ```
>
> ```
> 输入: n = 10
> 输出: 36
> 解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
> ```



#### 自己想法

1. 动态规划
2. `dp`数组的含义，需要想明白

```c++
class Solution {
public:
    int integerBreak(int n) {
        //dp数组的含义：要求啥就是啥，这里是要求乘积
        //dp[i]为i可以拆分得到的最大乘积
        //初始化 dp[2]=1
        //递推公式：dp[i]= i*(j-i)  或者 j*dp[i-j] 或者dp[i]本身
        vector<int> dp(n+1);

        dp[2]=1;
        for(int i=3;i<=n;i++){
            for(int j=1;j<=i/2;j++){
                dp[i] = max(dp[i], max(j*(i-j), j*dp[i-j]));
            }
        }
        return dp[n];
    }
};
```



#### 总结

1. `dp`数组的含义：要求啥就是啥