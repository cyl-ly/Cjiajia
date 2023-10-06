#### [长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

> 给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**
>
> 找出该数组中满足其总和大于等于 `target` 的长度最小的 **连续子数组** `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。

> ```
> 输入：target = 7, nums = [2,3,1,2,4,3]
> 输出：2
> 解释：子数组 [4,3] 是该条件下的长度最小的子数组。
> ```
>
> ```
> 输入：target = 4, nums = [1,4,4]
> 输出：1
> ```
>
> ```
> 输入：target = 11, nums = [1,1,1,1,1,1,1,1]
> 输出：0
> ```



#### 自己想法

1. 怎么去改变起始位置是关键

```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        //滑动窗口的思路，遍历的j是代表当前终止位置下标
        //当集合内sum达到target时，就找到了第一个符合条件的终止位置
        //所以开始随手调整起始位置
        //不断减去sum[i]，大于target就还符合要求，保留改动即可
        int i = 0;
        int sum = 0;
        int cnt = INT_MAX;
        for(int j=0; j<nums.size(); j++){
            sum += nums[j];
            if(sum >= target){
                int length = j-i+1;
                //cnt = min(cnt, length);
                while(sum-nums[i] >= target){   //调整起始位置
                    sum -= nums[i++];
                    length--;
                }
                cnt = min(cnt, length);
            }
        }
        return cnt==INT_MAX ? 0:cnt;
    }
};
```

