> 给你一个长度为 n 的整数数组 nums ，请你判断在 最多 改变 1 个元素的情况下，该数组能否变成一个非递减数列。
>
> 我们是这样定义一个非递减数列的： 对于数组中任意的 i (0 <= i <= n-2)，总满足 nums[i] <= nums[i + 1]
>
> ```
> 输入: nums = [4,2,3]
> 输出: true
> 解释: 你可以通过把第一个 4 变成 1 来使得它成为一个非递减数列。
> ```
>
> ```
> 输入: nums = [4,2,1]
> 输出: false
> 解释: 你不能在只改变一个元素的情况下将其变为非递减数列
> ```
>
> ```
> [3,4,2,3]     //都是很棒的测试案例
> [-1,4,2,3]    
> [5,7,1,8]
> ```



## 自己想法

> 真没完全想明白，案例通过321/335

1. 一开始思路是，把满足条件的位置标记为0，不满足条件的标记为1，但是这样即使是true的，也会有两个1，很快就pass了
2. 后面，想增加一个判断条件，就是记录走过的最大值，当前数字你还要大于前面的最大值，才算满足，反正例子很快又pass了
3. 想到了贪心，我边走边改不符合条件的数字，把不符合条件的位置改成前后一样的数字，这样至少满足=号，这就是到了321个案例，再往后面想想不下去了，直接看题解了

## 查看题解

1. 首先，我觉得flag的作用，和sum相同，但是有一点很重要的区别，**前两个元素直接单独判断**，省去了插入元素凑成“三个”这样的组合

2. 第二点就是让我觉得学习的点

   之前的案例，就是因为，有些是修改前者，有些是修改后者，我得方法无法进行兼容，而这里并没有想到能够合并在一起的方法

   > [3,4,2,3]和[5,7,1,8]
   >
   > 这两个真的是很棒的例子，一个修改不满足条件的前者，一个修改不满足条件的后者

3. 想法的关键点在哪，就是三个比较的数，一定有两个之间是符合条件的

   `[a,b,c]`，遍历到`b`了，下标为`i`，**已经有`a<b`的条件存在了（很关键）**

   那么此时有两种可能，一个是需要修改`b`，一个是需要修改`c`

   怎么考虑呢？

4. 如果`a<=c`的话，那么`b=c`，一定不会打乱顺序，符合条件

   如果`a>c`的话，因为`ab`之间关系已经正确了，那么需要修改的一定是`c`

```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) 
    {
        if (nums.size() == 1)   return true;
        bool flag = nums[0] <= nums[1] ? true : false; // 标识是否还能修改
        // 遍历时，每次需要看连续的三个元素
        for (int i = 1; i < nums.size() - 1; i++)
        {
            if (nums[i] > nums[i + 1])  // 出现递减
            {
                if (flag)   // 如果还有修改机会，进行修改
                {
                    if (nums[i + 1] >= nums[ i - 1])// 修改方案1
                        nums[i] = nums[i + 1];
                    else                            // 修改方案2
                        nums[i + 1] = nums[i];      
                    flag = false;                   // 用掉唯一的修改机会
                }   
                else        // 没有修改机会，直接结束
                    return false;
            }
        }
        return true;
    }
};
```

## 总结

1. 目前遇到的第一个没成功的题，能够轻松处理特殊位置、或者情况，总结一般规律的能力还需要锻炼

