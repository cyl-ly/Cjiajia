## [连续数组](https://leetcode.cn/problems/contiguous-array/description/)

> 给定一个二进制数组 `nums` , 找到含有相同数量的 `0` 和 `1` 的最长连续子数组，并返回该子数组的长度。

> ```
> 输入: nums = [0,1]
> 输出: 2
> 说明: [0, 1] 是具有相同数量 0 和 1 的最长连续子数组。
> ```
>
> ```
> 输入: nums = [0,1,0]
> 输出: 2
> 说明: [0, 1] (或 [1, 0]) 是具有相同数量0和1的最长连续子数组。
> ```

#### 题解

```c++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        //0看成-1 就是求连续子数组，和为0
        //sum[j]-sum[i-1]=0
        //sum[j] == sum[i-1]就行，哈希表记录第一次出现的位置，数组长度为j-i+1，也就是j-(i-1)，直接用下标去减就可以
        //但是这里的sum[j]是包含j的前缀和，sum[i-1]是不包含i的前缀和
        //也就是说，用下标i-1去存储i的前缀和（不包含i的）
        //哈希表存储是(前缀和，下标)

        unordered_map<int,int> mp;
        int pre_sum = 0;
        mp[0] = -1;  //前面啥也没有
        int length = 0;
        for(int i=0; i<nums.size(); i++){
            pre_sum = nums[i] == 0 ? pre_sum-1 : pre_sum+1;
            //包含当前下标的前缀和
            if(mp.find(pre_sum) != mp.end()){
                length = max(length, i-mp[pre_sum]);
            }
            else
                mp[pre_sum] = i;
        }
        return length;
    }
};
```

