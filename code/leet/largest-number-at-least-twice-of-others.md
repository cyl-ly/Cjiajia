### [至少是其他数字两倍的最大数](https://leetcode.cn/problems/largest-number-at-least-twice-of-others/)

> 给你一个整数数组 nums ，其中总是存在 唯一的 一个最大整数 。
>
> 请你找出数组中的最大元素并检查它是否 至少是数组中每个其他数字的两倍 。如果是，则返回 最大元素的下标 ，否则返回 -1
>
> ```
> 输入：nums = [3,6,1,0]
> 输出：1
> 解释：6 是最大的整数，对于数组中的其他整数，6 至少是数组中其他元素的两倍。6 的下标是 1 ，所以返回 1
> ```
>
> ```
> 输入：nums = [1,2,3,4]
> 输出：-1
> 解释：4 没有超过 3 的两倍大，所以返回 -1
> ```
>
> ```
> 输入：nums = [1]
> 输出：0
> 解释：因为不存在其他数字，所以认为现有数字 1 至少是其他数字的两倍
> ```



## 自己想法

> 执行用时：0 ms, 在所有 C++ 提交中击败了100.00% 的用户
>
> 内存消耗：10.7 MB, 在所有 C++ 提交中击败了11.78% 的用户

```c++
class Solution {
public:
    int dominantIndex(vector<int>& nums) {
        if(nums.size()==1)
            return 0;
        
        int max = *max_element(nums.begin(), nums.end());     //最大值
        int index = find(nums.begin(),nums.end(),max)-nums.begin();     //下标
        
        for(int i=0;i<nums.size();i++){ 
            if(nums[i]>max/2&&i!=index){   
                index=-1;      //有其他的大于max的一半，就是错误
                break;
            }
        }
        return index;
    }
};
```

1. 思路不难，就直接借鉴了一下，vector的最值和下标的简单语句，然后慢慢找就可以了

2. 但是，案例中有一个[1]，我在测试的时候，它提示我，我也不知道为啥会这样

   > `expected 'nums' to have 2 <= size <= 50 but got 1`

   

## 查看题解

1. 一边遍历，一边寻找，我一开始也是这个思路，但是没太深想，这个内存少了0.3MB，但是却击败了99%，哭泣啊

2. 看完代码想了一下，我是为什么没深入呢，就是都想把语句放入`if`和`else`里面了，在`for`里面，所有条件的外面也可以放语句的呀，这点忽略了，不应该

   ```c++
   class Solution {
   public:
       int dominantIndex(vector<int>& nums) {
           int mx = 0, ans = -1;
           for (int i = 0; i < nums.size(); ++i) {
               if (nums[i] >= mx * 2) ans = i;		//记录满足的下标
               else if (nums[i] * 2 > mx) ans = -1;	//不满足条件的-1
               mx = max(nums[i], mx);		//更新最大值，就是这里没想到放语句
           }
           return ans;
       }
   };
   ```

   

## 总结

1. ```c++
   int max = *max_element(nums.begin(), nums.end());     //最大值
   int index = find(nums.begin(),nums.end(),max)-nums.begin();     //下标
   ```

2. 思考清楚，初值是正常的话，就要思考异常处理条件，反之同理

3. `for`里面，所有`if`的外面，也可以放语句呀