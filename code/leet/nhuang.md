## [N皇后](https://leetcode.cn/problems/n-queens/description/)

> 按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。
>
> **n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。
>
> 给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。
>
> 每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

> ```
> 输入：n = 4
> 输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
> 解释：如上图所示，4 皇后问题存在两个不同的解法。
> ```
>
> ```
> 输入：n = 1
> 输出：[["Q"]]
> ```



#### 自己想法

1. 每次深入一层就是进入第一行，本身的for循环已经控制不在一行了

```c++
class Solution {
public:
    vector<vector<string>> result;  //一堆棋盘
    //之前是用一个单独变量sum或者path看是否合法，这里就需要一个函数了
    bool isvaild(vector<string> board, int row, int col, int n){    //都是下标
        //现在检查的是新放入的位置是否合法，只需要检查已经存在的和现在是否冲突就可以
        //同行不用检查，因为第一层for循环已经区分开了

        //还有已经走到这里了，证明以前的步骤都是合法的了
        //同时，要向上检查，已经上面是已经放置皇后的了
        for(int i=row-1; i>=0; i--)    //从这以上的列
            if(board[i][col] == 'Q')
                return false;
        for(int i=row-1, j=col-1; i>=0 && j>=0; i--, j--)     //正对角线
            if(board[i][j] == 'Q')
                return false;
        for(int i=row-1, j=col+1; i>=0 && j<n; i--, j++)        //斜对角线
            if(board[i][j] == 'Q')
                return false;
        return true;     
    }

    void backtracking(vector<string> board, int n, int row){   //row从0开始
        //终止条件，就是为了去保存结果的
        if(row == n){
            result.push_back(board);    //保存
            return;
        }

        //遍历一层
        for(int col=0; col<n; col++){
            if( isvaild(board, row, col, n) ){
                board[row][col] = 'Q';
                backtracking(board, n, row+1);
                board[row][col] = '.';    //回溯
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        result.clear();

        vector<string> board(n, string(n, '.'));
        backtracking(board, n, 0);
        return result;
    }
};
```

