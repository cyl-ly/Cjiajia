## [最大的以1为边界的正方形](https://leetcode.cn/problems/largest-1-bordered-square/description/)

> 给你一个由若干 `0` 和 `1` 组成的二维网格 `grid`，请你找出边界全部由 `1` 组成的最大 **正方形** 子网格，并返回该子网格中的元素数量。如果不存在，则返回 `0`。

> ```
> 输入：grid = [[1,1,1],[1,0,1],[1,1,1]]
> 输出：9
> ```
>
> ```
> 输入：grid = [[1,1,0,0]]
> 输出：1
> ```

#### 题解

1. 二维前缀和变形

```c++
class Solution {
public:
    int sum[102][102];  //补0
    void build(vector<vector<int>> &grid, int n ,int m){
        for(int i=1; i<=n; i++){
            for(int j=1; j<=m; j++){
                sum[i][j] = grid[i-1][j-1];     //复制
                sum[i][j] += (sum[i-1][j] + sum[i][j-1] - sum[i-1][j-1]);     //前缀和数组计算完毕
            }
        }
    }
    int sum_num(int a, int b, int c, int d){
        return sum[c][d] - sum[c][b-1] - sum[a-1][d] + sum[a-1][b-1];
        //即使补0了，但如果传入的是下标，也不用整体+1
    }
    int largest1BorderedSquare(vector<vector<int>>& grid) {
        int n = grid.size();    //行
        int m = grid[0].size();     //列
        build(grid, n, m);        //构建数组
        if(sum_num(1,1,n,m) == 0)   //整体都没有出现1
            return 0;

        //开始枚举正方形
        int cnt = 1;
        for(int a=1; a<=n; a++){    
            for(int b=1; b<=m; b++){
                //(a,b)
                //      (c,d)

                //找边长为(cnt+1)的正方形，但下标是增加cnt
                int index = cnt+1;
                for(int c=a+cnt, d=b+cnt; c<=n && d<=m; c++, d++, index++){
                    //找到符合要求的就保存结果，没找到就按正方形扩增
                    if(sum_num(a,b,c,d) - sum_num(a+1,b+1,c-1,d-1) == (index-1) << 2)   //(index-1)*4
                        cnt = index;  //保存结果，并去找比他大的
                }
            }
        }
        return cnt*cnt;
    }
};
```

