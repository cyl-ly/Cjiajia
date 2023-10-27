## [和为k的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/description/)

> 给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。
>
> 子数组是数组中元素的连续非空序列。

> ```
> 输入：nums = [1,1,1], k = 2
> 输出：2
> ```
>
> ```
> 输入：nums = [1,2,3], k = 3
> 输出：2
> ```

#### 题解

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        //先统计前缀和的数组，然后挨个去遍历，也算是N2，超时了
        
        //因为数组不是有序的，边遍历的时候，变计算前缀和，同时哈希存储出现次数
        //遍历到i时，找哈希中[k-pre_sum[i]]出现的次数，

        unordered_map<int,int> mp;
        mp[0] = 1;  //下标为0的前缀和为0，且出现1次
        int sum = 0;
        int result = 0;
        for(auto i : nums){
            sum += i;
            if(mp.find(sum-k) != mp.end())
                result += mp[sum-k];
            mp[sum]++;            
        }
        return result;
    }
};
```

