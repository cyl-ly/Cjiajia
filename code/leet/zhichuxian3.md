## [只出现一次的数字3](https://leetcode.cn/problems/single-number-iii/)

> 给你一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。你可以按 **任意顺序** 返回答案。
>
> 你必须设计并实现线性时间复杂度的算法且仅使用常量额外空间来解决此问题。

> ```
> 输入：nums = [1,2,1,3,2,5]
> 输出：[3,5]
> 解释：[5, 3] 也是有效的答案。
> ```
>
> ```
> 输入：nums = [-1,0]
> 输出：[-1,0]
> ```
>
> ```
> 输入：nums = [0,1]
> 输出：[1,0]
> ```



#### 自己想法

1. 异或以及提取出最右侧的1

```c++
class Solution {
public:
    vector<int> result;
    vector<int> singleNumber(vector<int>& nums) {
        long long num = 0;
        for(int i=0; i<nums.size(); i++)
            num ^= nums[i];     //此时为a^b，不会等于0，说明num里面至少有一位是1，也就是a和b在这一位是不同的
            //进而从这一位入手，把数组分成了两类
            //一类此位为1，一类为0，且ab不会在同一类
       
        long long rightone = num & (~num+1);
        //可以提取到右侧的第一个1，原码&补码
        //有数据取反+1，会超出int范围

        int num2 = 0;
        for(auto i:nums){
            if((i&rightone) == 0)
                num2 ^= i;      //rightone也可以，反正就是只异或一类数据
                //此时得到a or b
        }
        result.push_back(num2);
        num = num ^ num2;
        result.push_back(num);
        return result;
    }
};
```

