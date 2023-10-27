## [下一个更大的元素II](https://leetcode.cn/problems/next-greater-element-ii/description/)

> 给定一个循环数组 `nums` （ `nums[nums.length - 1]` 的下一个元素是 `nums[0]` ），返回 *`nums` 中每个元素的 **下一个更大元素*** 。
>
> 数字 `x` 的 **下一个更大的元素** 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 `-1` 。

> ```
> 输入: nums = [1,2,1]
> 输出: [2,-1,2]
> 解释: 第一个 1 的下一个更大的数是 2；
> 数字 2 找不到下一个更大的数； 
> 第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
> ```
>
> ```
> 输入: nums = [1,2,3,4,3]
> 输出: [2,3,4,-1,4]
> ```

#### 题解

```c++
class Solution {
public:
    int stack[20001];   //模拟栈
    int cur = 0;        //栈顶
    int result[20001];  //保存结果
    vector<int> nextGreaterElements(vector<int>& nums) {
        //把数组赋值两份，去寻找，而且还不用有检查阶段，因为前半截一定都会找到
        //而且发现，当相同的元素不让他出栈的时候，也是不用去包含检查阶段的

        vector<int> a = nums;
        a.insert(a.end(), nums.begin(), nums.end());
        //遍历阶段
        for(int i=0; i<a.size()-1; i++){
            while(cur > 0 && a[stack[cur-1]] < a[i]){
                int index = stack[--cur];
                result[index] = i;
            }
            stack[cur++] = i;
        }
        //清算阶段
        while(cur > 0){
            int index = stack[--cur];
            result[index] = -1;
        }
        
        //保存题目的结果
        vector<int> cnt(nums.size());
        for(int i=0; i<nums.size(); i++){
            cnt[i] = result[i] == -1 ? -1 : a[result[i]];
        }
        return cnt;
    }
};
```

