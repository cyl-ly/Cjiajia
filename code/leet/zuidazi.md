## [最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)

> 给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
>
> **子数组** 是数组中的一个连续部分。

> ```
> 输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出：6
> 解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
> ```
>
> ```
> 输入：nums = [1]
> 输出：1
> ```



#### 自己想法

1. 动态规划over

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        //dp数组含义：到当前下标为止，最大的连续数组和
        //迭代计算，加上本身会变大就加
        //如果不会就保留nums[i]
        //保留的是，从这里重新开始，或者说延续数组
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        for(int i=1; i<nums.size(); i++){
            dp[i] = max(nums[i], dp[i-1]+nums[i]);
        }
        auto max = max_element(dp.begin(), dp.end());
        return *max;
    }
};
```

