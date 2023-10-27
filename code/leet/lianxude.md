## [连续的子数组和*](https://leetcode.cn/problems/continuous-subarray-sum/description/)

> 给你一个整数数组 `nums` 和一个整数 `k` ，编写一个函数来判断该数组是否含有同时满足下述条件的连续子数组：
>
> - 子数组大小 **至少为 2** ，且
> - 子数组元素总和为 `k` 的倍数。
>
> 如果存在，返回 `true` ；否则，返回 `false` 。
>
> 如果存在一个整数 `n` ，令整数 `x` 符合 `x = n * k` ，则称 `x` 是 `k` 的一个倍数。`0` 始终视为 `k` 的一个倍数。

> ```
> 输入：nums = [23,2,4,6,7], k = 6
> 输出：true
> 解释：[2,4] 是一个大小为 2 的子数组，并且和为 6 。
> ```
>
> ```
> 输入：nums = [23,2,6,4,7], k = 6
> 输出：true
> 解释：[23, 2, 6, 4, 7] 是大小为 5 的子数组，并且和为 42 。 
> 42 是 6 的倍数，因为 42 = 7 * 6 且 7 是一个整数。
> ```
>
> ```
> 输入：nums = [23,2,6,4,7], k = 13
> 输出：false
> ```

#### 题解

1. 先用数学去分析很重要的

```c++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        //所求区间为i-j，就是sum[j]-sum[i-1] = nk，
        //把k除过去，就是对sumj，i 对k取余数相同(性质)

        //要求长度是2，遍历的时候用i-2控制距离

        vector<int> sum(nums.size()+1);
        sum[0] = 0;
        for(int i=1; i<=nums.size(); i++){
            sum[i] = sum[i-1]+nums[i-1];
        }   //前缀和
        unordered_set<int> st;  //存放取余数的集合，遍历到i的时候，不断放入长度向前数两个的前缀和取余数
        //区间要求的是sum[j]-sum[i-1]才是包含i-j的所有和
        for(int i=2; i<=nums.size(); i++){
            //sum[2]-sum[0]才是这个区间的和
            st.insert(sum[i-2] % k);
            if(st.count(sum[i] % k))
                return true;
        }
        return false;
    }
};
```

