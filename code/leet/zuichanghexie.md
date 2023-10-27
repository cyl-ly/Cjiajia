## [最长和谐子序列](https://leetcode.cn/problems/longest-harmonious-subsequence/submissions/477203362/)

> 和谐数组是指一个数组里元素的最大值和最小值之间的差别 **正好是 `1`** 。
>
> 现在，给你一个整数数组 `nums` ，请你在所有可能的子序列中找到最长的和谐子序列的长度。
>
> 数组的子序列是一个由数组派生出来的序列，它可以通过删除一些元素或不删除元素、且不改变其余元素的顺序而得到。

> ```
> 输入：nums = [1,3,2,2,5,2,3,7]
> 输出：5
> 解释：最长的和谐子序列是 [3,2,2,2,3]
> ```
>
> ```
> 输入：nums = [1,2,3,4]
> 输出：2
> ```
>
> ```
> 输入：nums = [1,1,1,1]
> 输出：0
> ```

#### 题解

1. 在没确定map元素存在之前，任何出现的下标访问语句都会导致创建元素，并置为0

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        //动态规划？？原来不是要求必须连续的
        unordered_map<int,int> mp;
        for(auto i:nums)    mp[i]++;

        int result = 0;
        for(auto i:mp){
            int sum = i.second;
            int num1 = mp.find(i.first-1) == mp.end() ? 0 : mp[i.first-1];
            int num2 = mp.find(i.first+1) == mp.end() ? 0 : mp[i.first+1];
            sum += max(num1, num2);
            if(num1 == 0 && num2 == 0)
                continue;
            result = max(result, sum);
            
        }
        return result;
        
    }
};
```

