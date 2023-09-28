#### [有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

> 给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

> ```
> 输入：nums = [-4,-1,0,3,10]
> 输出：[0,1,9,16,100]
> 解释：平方后，数组变为 [16,1,0,9,100]
> 排序后，数组变为 [0,1,9,16,100]
> ```
>
> ```
> 输入：nums = [-7,-3,2,3,11]
> 输出：[4,9,9,49,121]
> ```



#### 自己想法

1. 最大值一定是在两边取到，双指针，不断向中间缩小

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n = nums.size();
        vector<int> b(n);
        int k = n-1;
        int i = 0;
        int j = nums.size()-1;
        while(i<=j){
            int num1 = nums[i]*nums[i];
            int num2 = nums[j]*nums[j];
            if(num1>num2){
                b[k--] = num1;
                i++;
            }
            else if(num1<num2){
                b[k--] = num2;
                j--;
            }
            else if(i==j){
                b[k--] = num1;
                i++;
                j--;
            }
            else{
                b[k--] = num1;
                b[k--] = num1;
                i++;
                j--;
            }
        }
        //b.insert(b.begin(), nums[i]*nums[i]);
        return b;
    }
};
```

