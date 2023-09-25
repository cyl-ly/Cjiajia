#### [分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

> 给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

> ```
> 输入：nums = [1,5,11,5]
> 输出：true
> 解释：数组可以分割成 [1, 5, 5] 和 [11] 。
> ```
>
> ```
> 输入：nums = [1,2,3,5]
> 输出：false
> 解释：数组不能分割成两个元素和相等的子集。
> ```



#### 自己想法

1. 将看似不是背包问题的转移过来
2. 与01背包问题，对应的元素数组要怎么找到确定

```c++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        //两个集合的总和已知可求，就是往一个里面装元素，可以转化为0-1背包
        //供装入的物品一共有n个，nums[i]为物品的价值，还缺少重量信息
        //dp[j]即为价值，和sum相等，那么就是符合要求的
        //如果重量设置为1，那么容量设置为多少合适，两个背包内的个数并不一定是相等的，或者是已知可求的
        //考虑把物品的重量和价值都定为nums[i]
        //这样总数就是背包的容量j，关键点！！！
        //相当于把重量和价值画等号了，要达到sum的价值，就是达到sum的j
        int n=nums.size();
        int sum2=accumulate(nums.begin(), nums.end(), 0);
        int sum=sum2/2;     //背包的最大容量
        if(sum2%2 != 0)         //和是奇数，一定false，但长度是奇数，不一定false，没要求个数均分
            return false;

        vector<int> dp(sum+1);
        dp[0]=0;
        for(int i=0; i<n; i++){     //遍历物品
            for(int j=sum; j>=nums[i]; j--){    //遍历背包，要注意截止条件，和一维时，逆向遍历
                dp[j]=max(dp[j], dp[j-nums[i]]+nums[i]);             
            }
        }
        if(dp[sum] == sum)
            return true;
        return false;
    }
};
```



#### 总结

1. 缺少重量和价值之一时，要怎么考虑