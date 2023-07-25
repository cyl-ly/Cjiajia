> 给你一个整型数组 `nums` ，在数组中找出由三个数组成的最大乘积，并输出这个乘积
>
> ```
> 输入：nums = [1,2,3]
> 输出：6
> ```
>
> ```
> 输入：nums = [1,2,3,4]
> 输出：24
> ```
>
> ```
> 输入：nums = [-1,-2,-3]
> 输出：-6
> ```

## 自己想法

> 执行用时：72 ms, 在所有 C++ 提交中击败了6.14% 的用户
>
> 内存消耗：27 MB, 在所有 C++ 提交中击败了75.57% 的用户

```c++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {

        sort(nums.rbegin(),nums.rend());
        int n1=nums[0]*nums[1]*nums[2];
        int n2=nums[0]*nums[nums.size()-1]*nums[nums.size()-2];
        if(n1>n2)  
            return n1;
        return n2;
    }
};
```

这个题感觉没啥好说的呀，排好序之后，三个正数，或者两负一正，必然最大，相比较就可以了

## 查看题解

1. 题解好像有提到并查集，数据结构还没看，以后再说，而且这题简单思路就很简单，略过

## 总结

无