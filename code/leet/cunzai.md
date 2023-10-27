## [存在重复元素II](https://leetcode.cn/problems/contains-duplicate-ii/submissions/477190503/)

> 给你一个整数数组 `nums` 和一个整数 `k` ，判断数组中是否存在两个 **不同的索引** `i` 和 `j` ，满足 `nums[i] == nums[j]` 且 `abs(i - j) <= k` 。如果存在，返回 `true` ；否则，返回 `false` 。

> ```
> 输入：nums = [1,2,3,1], k = 3
> 输出：true
> ```
>
> ```
> 输入：nums = [1,0,1,1], k = 1
> 输出：true
> ```
>
> ```
> 输入：nums = [1,2,3,1,2,3], k = 2
> 输出：false
> ```

#### 题解

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        if(k == 0)
            return false;
        unordered_map<int, int> mp;
        for(int i=0; i<nums.size(); i++){
            if(mp.find(nums[i]) != mp.end() && abs(i-mp[nums[i]]) <= k)
                return true;    //边存储，边去找是否符合条件，
            mp[nums[i]] = i;
        }
        return false;
    }
};
```

