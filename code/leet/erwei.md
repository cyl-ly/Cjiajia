## [二维区域和检索-矩阵不可变](https://leetcode.cn/problems/range-sum-query-2d-immutable/description/)

> 给定一个二维矩阵 `matrix`，以下类型的多个请求：
>
> - 计算其子矩形范围内元素的总和，该子矩阵的 **左上角** 为 `(row1, col1)` ，**右下角** 为 `(row2, col2)` 。
>
> 实现 `NumMatrix` 类：
>
> - `NumMatrix(int[][] matrix)` 给定整数矩阵 `matrix` 进行初始化
> - `int sumRegion(int row1, int col1, int row2, int col2)` 返回 **左上角** `(row1, col1)` 、**右下角** `(row2, col2)` 所描述的子矩阵的元素 **总和** 。

> ```
> 输入: 
> ["NumMatrix","sumRegion","sumRegion","sumRegion"]
> [[[[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]],[2,1,4,3],[1,1,2,2],[1,2,2,4]]
> 输出: 
> [null, 8, 11, 12]
> ```
>
> ```
> 解释:
> NumMatrix numMatrix = new NumMatrix([[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]);
> numMatrix.sumRegion(2, 1, 4, 3); // return 8 (红色矩形框的元素总和)
> numMatrix.sumRegion(1, 1, 2, 2); // return 11 (绿色矩形框的元素总和)
> numMatrix.sumRegion(1, 2, 2, 4); // return 12 (蓝色矩形框的元素总和)
> ```

#### 题解

1. 二维前缀和

```c++
class NumMatrix {
public:
    int sum[202][202]={0};      //二维差分数组,补0
    NumMatrix(vector<vector<int>>& matrix) {
        int n = matrix.size();  //行
        int m = matrix[0].size();   //列
        for(int i=1; i<=n; i++){//从1行开始
            for(int j=1; j<=m; j++){
                sum[i][j] = matrix[i-1][j-1];
                sum[i][j] += (sum[i-1][j] + sum[i][j-1] - sum[i-1][j-1]);
                //可以单独复制一遍，也可以边复制，边计算
            }
        }
    }
    
    int sumRegion(int a, int b, int c, int d) {
        int result = 0;
        result = sum[c+1][d+1] - sum[c+1][b] - sum[a][d+1] + sum[a][b];     //要记住补0了
        return result;
    }
};
```

