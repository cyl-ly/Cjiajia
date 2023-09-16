#### [不同路径2](https://leetcode.cn/problems/unique-paths-ii/)

> 一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。
>
> 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。
>
> 现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？
>
> 网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

> ```
> 输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
> 输出：2
> 解释：3x3 网格的正中间有一个障碍物。
> 从左上角到右下角一共有 2 条不同的路径：
> 1. 向右 -> 向右 -> 向下 -> 向下
> 2. 向下 -> 向下 -> 向右 -> 向右
> ```
>
> ```
> 输入：obstacleGrid = [[0,1],[0,0]]
> 输出：1
> ```



#### 自己想法

1. 动态规划
2. 和不同路径区别只是有障碍物
3. 在障碍物处，将`dp[i][j]`设置为0，即可，其余不变
4. 还要考虑在初始化时，一旦有障碍物后，后续的行 or 列全部都是0

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        //dp[i][j]为到i，j点的不同路径
        //dp[][]初始化 第一行和第一列
        //dp的迭代计算方式
        //dp[i][j]=dp[i-1][j]+dp[i][j-1]，有障碍物的分情况讨论即可
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();  //m行n列

        int dp[m][n];
        int flag=-1;
        for(int i=0; i<m; i++){
            if(obstacleGrid[i][0] == 1) 
                flag=1;
            if(flag==-1)
                dp[i][0]=1;
            else   
                dp[i][0]=0;
        }
        flag=-1;
        for(int j=0; j<n; j++){
            if(obstacleGrid[0][j] == 1) flag=1;
            if(flag==-1)
                dp[0][j]=1;
            else   
                dp[0][j]=0;
        }//初始化一行和一列，并把障碍物考虑在内了

        for(int i=1; i<m; i++){
            for(int j=1; j<n; j++){
                if(obstacleGrid[i][j] == 1){      //此处是障碍物
                    dp[i][j]=0;
                }
                else
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

