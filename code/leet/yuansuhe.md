## [元素和最小的山形三元组II——周赛](https://leetcode.cn/problems/minimum-sum-of-mountain-triplets-ii/)

> 给你一个下标从 **0** 开始的整数数组 `nums` 。
>
> 如果下标三元组 `(i, j, k)` 满足下述全部条件，则认为它是一个 **山形三元组** ：
>
> - `i < j < k`
> - `nums[i] < nums[j]` 且 `nums[k] < nums[j]`
>
> 请你找出 `nums` 中 **元素和最小** 的山形三元组，并返回其 **元素和** 。如果不存在满足条件的三元组，返回 `-1` 。

> ```
> 输入：nums = [8,6,1,5,3]
> 输出：9
> 解释：三元组 (2, 3, 4) 是一个元素和等于 9 的山形三元组，因为： 
> - 2 < 3 < 4
> - nums[2] < nums[3] 且 nums[4] < nums[3]
> 这个三元组的元素和等于 nums[2] + nums[3] + nums[4] = 9 。可以证明不存在元素和小于 9 的山形三元组。
> ```
>
> ```
> 输入：nums = [5,4,8,7,10,2]
> 输出：13
> 解释：三元组 (1, 3, 5) 是一个元素和等于 13 的山形三元组，因为： 
> - 1 < 3 < 5 
> - nums[1] < nums[3] 且 nums[5] < nums[3]
> 这个三元组的元素和等于 nums[1] + nums[3] + nums[5] = 13 。可以证明不存在元素和小于 13 的山形三元组。
> ```
>
> ```
> 输入：nums = [6,5,4,3,4,5]
> 输出：-1
> 解释：可以证明 nums 中不存在山形三元组。
> ```



#### 自己想法

1. 枚举，一开始向去用之前三数之和的形式去用双向指针去做，发现不行，LR去移动谁无法实现精准判断

2. 稍微优化了一遍，前面的最小值可以动态更新记录的

```c++
class Solution {
public:
    int minimumSum(vector<int>& nums) {
        vector<int> min_after(nums.size());
        int result = INT_MAX;
        min_after[nums.size()-1] = 0, min_after[nums.size()-2] = nums[nums.size()-1];
        for(int i=nums.size()-3; i>=0; i--)     //先维护好后面的min
            min_after[i] = min(min_after[i+1], nums[i+1]);
        int min_before = nums[0];   //遍历时动态记录前面的最小值

        for(int i=1; i<nums.size()-1; i++){
            //先判断是否满足条件，再去更新min
            if(nums[i] > min_before && nums[i] > min_after[i]){
                int sum = nums[i]+min_after[i]+min_before;
                result = min(result, sum);
            }
            min_before = min(nums[i], min_before);
        }
        return result == INT_MAX ? -1 : result;
    }
};
```

