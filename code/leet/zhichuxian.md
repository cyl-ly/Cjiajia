## [只出现一次的数字](https://leetcode.cn/problems/single-number/)

> 给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>
> 你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

> ```
> 输入：nums = [2,2,1]
> 输出：1
> ```
>
> ```
> 输入：nums = [4,1,2,1,2]
> 输出：4
> ```
>
> ```
> 输入：nums = [1]
> 输出：1
> ```



#### 自己想法

1. 异或运算性质

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int eor = nums[0];
        for(int i=1; i<nums.size(); i++){
            eor = eor^nums[i];
        }
        return eor;
    }
};
```

