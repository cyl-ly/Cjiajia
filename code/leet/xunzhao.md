## [寻找峰值](https://leetcode.cn/problems/find-peak-element/description/)

> 峰值元素是指其值严格大于左右相邻值的元素。
>
> 给你一个整数数组 `nums`，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 **任何一个峰值** 所在位置即可。
>
> 你可以假设 `nums[-1] = nums[n] = -∞` 。
>
> 你必须实现时间复杂度为 `O(log n)` 的算法来解决此问题。

> ```
> 输入：nums = [1,2,3,1]
> 输出：2
> 解释：3 是峰值元素，你的函数应该返回其索引 2。
> ```
>
> ```
> 输入：nums = [1,2,1,3,5,6,4]
> 输出：1 或 5 
> 解释：你的函数可以返回索引 1，其峰值元素为 2；
>   或者返回索引 5， 其峰值元素为 6。
> ```



#### 自己想法

1. 二分法应用，soeasy

```c++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        //这个长度只是>=1，还是先判断0 n-1 位置

        if(nums.size() == 1)
            return 0;
        if(nums[0] > nums[1])
            return 0;
        int n = nums.size();
        if(nums[n-1] > nums[n-2])
            return n-1;

        int left = 0, right = n-1;
        while(left<=right){
            int mid = (left+right)/2;
            if(nums[mid] < nums[mid-1])
                right = mid;
            else if(nums[mid] < nums[mid+1])
                left = mid;
            else if(nums[mid] > nums[mid-1] && nums[mid] > nums[mid+1])
                return mid;
        }
        return 0;
    }
};
```

