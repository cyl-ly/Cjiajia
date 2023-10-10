## [三数之和](https://leetcode.cn/problems/3sum/)

> 给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请
>
> 你返回所有和为 `0` 且不重复的三元组。
>
> **注意：**答案中不可以包含重复的三元组。

> ```
> 输入：nums = [-1,0,1,2,-1,-4]
> 输出：[[-1,-1,2],[-1,0,1]]
> 解释：
> nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
> nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
> nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
> 不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
> 注意，输出的顺序和三元组的顺序并不重要。
> ```
>
> ```
> 输入：nums = [0,1,1]
> 输出：[]
> 解释：唯一可能的三元组和不为 0 。
> ```
>
> ```
> 输入：nums = [0,0,0]
> 输出：[[0,0,0]]
> 解释：唯一可能的三元组和为 0 。
> ```



#### 自己想法

1. 细节太多了，好好把握

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());     //对数组进行排序

        for(int i=0; i<nums.size(); i++){
            if(nums[0] > 0)
                return result;      //排好序，前面必须小于0才可能

            //对a如何去重，
            if(i>0 && nums[i]==nums[i-1]){      //如果是i和i+1，就会导致left和i相等的情况被忽略
                //和i-1相同，代表当前元素的组合结果，至少前面已经收集过了
                //{-1,-1,1}就被忽略了
                continue;
            }
            int left = i+1, right = nums.size()-1;
            while(left < right){//不能有等于，因为重复了
                int num = nums[i]+nums[left]+nums[right];
                if(num > 0)
                    right--;        //sum大于0，说明需要减小，右边就--
                else if(num < 0)
                    left++;
                else if(num == 0){
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    while(left<right && nums[right] == nums[right-1])    right--;
                    while(left<right && nums[left] == nums[left+1])      left++;
                
                //为什么是在后面去重，因为这样，至少保证了所在元素已经被收集了一遍结果集
                //先去重的话，都没有收集，可能就没了{0,0,0,0,0}
                    left++;     //上面while只是移动到了相同元素的最后一个，所以还需要移动一次
                                //或者不重复时，也需要移动一次
                    right--;
                }
            }
        }
        return result;
    }
};
```

