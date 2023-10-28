## [最大宽度坡](https://leetcode.cn/problems/maximum-width-ramp/description/)

> 给定一个整数数组 `A`，*坡*是元组 `(i, j)`，其中 `i < j` 且 `A[i] <= A[j]`。这样的坡的宽度为 `j - i`。
>
> 找出 `A` 中的坡的最大宽度，如果不存在，返回 0 。

> ```
> 输入：[6,0,8,2,1,5]
> 输出：4
> 解释：
> 最大宽度的坡为 (i, j) = (1, 5): A[1] = 0 且 A[5] = 5.
> ```
>
> ```
> 输入：[9,8,1,0,1,9,4,0,4,1]
> 输出：7
> 解释：
> 最大宽度的坡为 (i, j) = (2, 9): A[2] = 1 且 A[9] = 1.
> ```

#### 题解

```c++
class Solution {
public:
    int stack[50001];   //模拟栈
    int cur = 0;
    void compute(vector<int> &nums, int &cnt){
        //左到右遍历收集起点
        //最左边的点一定要进栈
        stack[cur++] = 0;
        for(int i=1; i<nums.size(); i++){
            if(cur > 0 && nums[i] < nums[stack[cur-1]]){
                stack[cur++] = i;   //小于栈顶才进栈
            }
        }
        //右到左遍历结算
        for(int i=nums.size()-1; i>=0; i--){
            while(cur > 0 && nums[i] >= nums[stack[cur-1]]){
                int index = stack[--cur];   //出栈+结算
                cnt = max(cnt, i-index);
            }
            //当栈中元素不在满足<=时，i--，
        }
    }
    int maxWidthRamp(vector<int>& nums) {
        //只要满足>=就是一个坡，求出最大宽度，
        //第一遍遍历中，我们就要保存可能是坡起点的可能点
        //只要有小于的就进栈，大于栈顶的全忽略，因为他不可能比栈顶更容易出答案
        int cnt = 0;
        compute(nums, cnt);
        return cnt;
    }
};
```

