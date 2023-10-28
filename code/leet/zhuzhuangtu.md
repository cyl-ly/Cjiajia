## [柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/description/)

> 给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
>
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。

> ```
> 输入：heights = [2,1,5,6,2,3]
> 输出：10
> 解释：最大的矩形为图中红色区域，面积为 10
> ```
>
> ```
> 输入： heights = [2,4]
> 输出： 4
> ```

#### 题解

```c++
class Solution {
public:
//单调栈
int stack[100001];
int cur = 0;
int result[100001][2];   //保存前面比他更小的，和后面比他更小的————(下标)
void compute(vector<int> &arr, int &mm){
    //遍历阶段
    for(int i=0; i<arr.size(); i++){
        while(cur > 0 && arr[stack[cur-1]] >= arr[i]){
            int index = stack[--cur];
            result[index][1] = i;       //后面比他小的，就是让他出栈的
            result[index][0] = cur > 0 ? stack[cur-1] : -1;    //前面比他小的，就是栈里面的邻居
            int num1 = (result[index][1] - result[index][0]-1)*arr[index];
            //当前的高度，去画长方形，不能覆盖到比自己小的区间上，你会超过他们
            mm = max(mm, num1);
        }
        stack[cur++] = i;
    }
    //清算阶段
    while(cur > 0){		//后面没有元素了，全出来
        int index = stack[--cur];
        result[index][1] = -1;      
        result[index][0] = cur > 0 ? stack[cur-1] : -1;
        int num1 = (arr.size() - result[index][0]-1)*arr[index];
        mm = max(mm, num1);
    }
    
}

    int largestRectangleArea(vector<int>& heights) {
        //有了左右都比他小的之后，以index为中心，左右进行扩展，求出面积，保存最大值
        //考虑相等是否弹出要去画图走一遍
        //不要着急，看相等时候，后面的元素弹出来，会不会能弥补空缺
        int mm = 0;
        compute(heights, mm);
        return mm;
    }
};
```

