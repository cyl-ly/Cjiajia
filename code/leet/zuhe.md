## [组合](https://leetcode.cn/problems/combinations/description/)

> 给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。
>
> 你可以按 **任何顺序** 返回答案。

> ```
> 输入：n = 4, k = 2
> 输出：
> [
>   [2,4],
>   [3,4],
>   [2,3],
>   [1,2],
>   [1,3],
>   [1,4],
> ]
> ```
>
> ```
> 输入：n = 1, k = 1
> 输出：[[1]]
> ```



#### 自己想法

1. 组合要与排列区分开，**组合没有顺序**

```
        1 			2
      2  3  4     3   4
      →↑ →↑ →↑
      return
```

1. 完全搜索方法，不能直接改变常数n，因为n是带入到下面每一层的

```c++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;   //存放搜索路径
    void backtracking(int n, int k, int findbegin){
        //一层递归就是一层for循环
        //手绘一下流程图，在叶子结点进行一个返回的函数，特别清晰

        //终止条件
        if(path.size() == k){
            result.push_back(path); //保存结果
            return;     //返回
        }

        //遍历
        for(int i=findbegin; i<=n; i++){    //树状的一层
            path.push_back(i);  
            backtracking(n, k, i+1);    //往下递归一层，就是一个for循环
            path.pop_back();    
            //进入节点遍历之后，就会达到终止条件，return，此时删掉直接进入下一个节点
            //这就是回溯的经典所在
        }
    }
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```

2. 剪枝优化版本

```c++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;   //存放搜索路径
    void backtracking(int n, int k, int findbegin){
        //终止条件
        if(path.size() == k){
            result.push_back(path); //保存结果
            return;     //返回
        }
        //遍历
        for(int i=findbegin; i<=n+1-k+path.size(); i++){    //树状的一层
            path.push_back(i);  
            backtracking(n, k, i+1);    //往下递归一层，就是一个for循环
            path.pop_back();    
            //剪枝优化版本
            //k - path.size()是还需要找到的个数
            //那么n-i+1至少要>=k-path.size()
            //i<= n+1-k+path.size()
        }
    }
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```

