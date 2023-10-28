## [最大矩形](https://leetcode.cn/problems/maximal-rectangle/description/)

> 给定一个仅包含 `0` 和 `1` 、大小为 `rows x cols` 的二维二进制矩阵，找出只包含 `1` 的最大矩形，并返回其面积。

> ```
> 输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
> 输出：6
> 解释：最大矩形如上图所示。
> ```
>
> ```
> 输入：matrix = []
> 输出：0
> ```
>
> ```
> 输入：matrix = [["0","0"]]
> 输出：0
> ```

#### 题解

1. 单调栈的变形，每层去遍历，弹出结算

```c++
class Solution {
public:
    int stack[201];     //栈
    int cur = 0;
    int result[201][2];     //单调栈结果
    void compute(vector<int> &nums, int &cnt){    //每次传入一层
        //遍历阶段
        for(int i=0; i<nums.size(); i++){
            while(cur > 0 && nums[stack[cur-1]] >= nums[i]){    
                //相等时出栈，后面会补充上，而且不用去检验阶段
                int index = stack[--cur];
                result[index][0] = cur > 0 ? stack[cur-1] : -1;
                result[index][1] = i;
                //result[index][0]  index   result[index][1]   三个点
                int len = result[index][1] - result[index][0] - 1;
                cnt = max(cnt, len*nums[index]);    //能拓展到的宽度*本身高度
            }
            stack[cur++] = i;
        }
        //清算阶段
        while(cur > 0){
            int index = stack[--cur];
            result[index][0] = cur > 0 ? stack[cur-1] : -1;
            result[index][1] = nums.size();
            int len = result[index][1] - result[index][0] - 1;
            cnt = max(cnt, len*nums[index]);
        }
        //检验阶段省略
    }
    int maximalRectangle(vector<vector<char>>& matrix) {
        //以每层为底，进入到单调栈中，根据左右更小的确定出栈元素的面积
        vector<int> nums(matrix[0].size(), 0);       //每一层的压缩数组
        int cnt = 0;
        int result = 0;
        for(int i=0; i<matrix.size(); i++){     //i是行数
            for(int j=0; j<matrix[0].size(); j++){
                nums[j] = matrix[i][j] == '1' ? nums[j]+1 : 0;  //为0就重置
            }
            compute(nums, cnt);
            result = max(result, cnt);
        }
        return result;
    }
};
```

