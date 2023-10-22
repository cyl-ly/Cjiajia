## [最接近的三数之和](https://leetcode.cn/problems/3sum-closest/submissions/476038536/)

> 给你一个长度为 `n` 的整数数组 `nums` 和 一个目标值 `target`。请你从 `nums` 中选出三个整数，使它们的和与 `target` 最接近。
>
> 返回这三个数的和。
>
> 假定每组输入只存在恰好一个解。

> ```
> 输入：nums = [-1,2,1,-4], target = 1
> 输出：2
> 解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
> ```
>
> ```
> 输入：nums = [0,0,0], target = 1
> 输出：0
> ```



#### 自己想法

1. 这个之前做过三数之和，差点忘了，有一个while(L<R)的细节差点忽略

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int sum = 0;
        int index = INT_MAX;
        int result = 0;
        sort(nums.begin(), nums.end());
        for(int i=0; i<nums.size()-2; i++){
            int L = i+1;        //L
            int R = nums.size()-1;  //最后一个数
            while(L < R){
                sum = nums[i]+nums[L]+nums[R];
                if(sum < target)    //小于L往右走
                    L++;
                else if(sum > target)
                    R--;
                else 
                    return target;

                if(abs(target-sum) < index){
                    index = abs(target-sum);
                    result = sum;
                }
            }
        }
        return result;
    }
};
```

