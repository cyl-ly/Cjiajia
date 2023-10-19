## N皇后变体——OJ

> 不能一行，一列放置
>
> 同时有墙挡着



#### 自己想法

1. 只放了关键代码
2. 这个可以实现一行多放，用除法取余数，巧妙控制

```c++
bool isvaild(vector<string> board, int row, int col, int n){	//下标
	//开始检查，X是墙
	if(board[row][col] == 'X')	//有墙，放不了
		return false;
	
	for(int i=row-1; i>=0; i--){		//以上的列
		if(board[i][col] == 'Q')
			return false;
		else if(board[i][col] == 'X')
			break;
	}
	for(int i=col-1; i>=0; i--){		//同行以前检查
		if(board[row][i] == 'Q')
			return false;
		else if(board[row][i] == 'X')
			break;
	}
	return true;
	
}

void backtracking(vector<string> board, int n, int index){
	//终止条件
	if(index == n*n){		//这里有些情况是第3行没有符合条件的，无法进入深一层的递归
		//prf(board, n);
		return;
	}
	//遍历
	int x = index / n;	//行数
	int y = index % n;	//列数

	if(isvaild(board, x, y, n)){
		board[x][y] = 'Q';
		getsum(board, n);	//保存
		//prf(board, n);
		backtracking(board, n, index+1);	//递归进入右侧下一个位置，满一行换行
		board[x][y] = '.';	//回溯
	}
	backtracking(board, n, index+1);	//当前不能开始，平行进入右侧下一个位置
}
```

