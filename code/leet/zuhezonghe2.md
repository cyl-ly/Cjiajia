## [组合总和2](https://leetcode.cn/problems/combination-sum-ii/)

> 给定一个候选人编号的集合 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。
>
> `candidates` 中的每个数字在每个组合中只能使用 **一次** 。
>
> **注意：**解集不能包含重复的组合。 

> ```
> 输入: candidates = [10,1,2,7,6,1,5], target = 8,
> 输出:
> [
> [1,1,6],
> [1,2,5],
> [1,7],
> [2,6]
> ]
> ```
>
> ```
> 输入: candidates = [2,5,2,1,2], target = 5,
> 输出:
> [
> [1,2,2],
> [5]
> ]
> ```



#### 自己想法

1. 剪枝：利用flag没深入一层就改变一下，用来区别是否同一层

```c++
class Solution {
public:
    vector<vector<int>> result;
    int sum;    //已经遍历节点的和
    vector<int> path;
    
    void backtracking(int target, int findbegin, vector<int> nums, vector<int> flag){ //开始位置为下标
        //终止条件，还要考虑到大于的情况
        if(sum == target){
            result.push_back(path); //保存
            return; //不能忘记
        if(sum > target)
            return;
        //遍历一层，&&后面的是剪枝，利用sum进行
        for(int i=findbegin; i<nums.size() && nums[i]<=target-sum; i++){
            //怎么剪枝，1,1,2,5,7的1,1 会重复遍历
            //要去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重
            //第一个思路，但是没搞定：if(i>0 && findbegin==0 && nums[i]==nums[i-1])   //去重第一层上的
            //只去重第一层，还是时间超时，要考虑去重每个同一层的

            if(i>0 && nums[i]==nums[i-1] && flag[i-1]==0)   //flag变为1就是同一树枝用过的，0就是同一层用过的
                continue;
            
            path.push_back(nums[i]);
            sum += nums[i];
            flag[i] = 1;
            backtracking(target, i+1, nums, flag);
            //回溯
            flag[i] = 0;
            path.pop_back();
            sum -= nums[i];
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        int num = accumulate(candidates.begin(), candidates.end(), 0);
        if(num < target)
            return result;
        
        vector<int> flag(candidates.size(), 0);
        sort(candidates.begin(), candidates.end());     //排序
        backtracking(target, 0, candidates, flag);    
        

        //怎么去重??——可以实现二维vector去重，但是时间超时，关键还是在剪枝
        //sort(result.begin(), result.end());
        //result.erase( unique(result.begin(), result.end()), result.end() );
        return result;
    }
};
```

2. 剪枝：利用遍历位置进行去重，同一层的相同元素，第一个去进行遍历回溯，保存结果，后序的进行去重，此时位置一定大于begin起始位置了

```c++
class Solution {
public:
    vector<vector<int>> result;
    int sum;    //已经遍历节点的和
    vector<int> path;
    
    void backtracking(int target, int findbegin, vector<int> nums, int flag[]){ //开始位置为下标
        //终止条件，还要考虑到大于的情况
        if(sum == target){
            result.push_back(path); //保存
            return; //不能忘记
        }
        if(sum > target)
            return;
        //遍历一层，&&后面的是剪枝，利用sum进行
        for(int i=findbegin; i<nums.size() && nums[i]<=target-sum; i++){
            //怎么剪枝，1,1,2,5,7的1,1 会重复遍历
            //要去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重
            //第一个思路，但是没搞定：if(i>0 && findbegin==0 && nums[i]==nums[i-1])   //去重第一层上的
            //只去重第一层，还是时间超时，要考虑去重每个同一层的

            if(i-findbegin>0 && nums[i]==nums[i-1] )   
                continue;
            //b站评论区参考的，i>findbegin就是保证了是同一层的去重，就是已经在同一层往后走了
            //因为一个树枝的已经在第一次的时候遍历过了
            
            path.push_back(nums[i]);
            sum += nums[i];
            flag[i] = 1;
            backtracking(target, i+1, nums, flag);
            //回溯
            flag[i] = 0;
            path.pop_back();
            sum -= nums[i];
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        int num = accumulate(candidates.begin(), candidates.end(), 0);
        if(num < target)
            return result;
        
        int flag[candidates.size()];
        sort(candidates.begin(), candidates.end());     //排序
        backtracking(target, 0, candidates, flag);    
        

        //怎么去重??——可以实现二维vector去重，但是时间超时，关键还是在剪枝
        //sort(result.begin(), result.end());
        //result.erase( unique(result.begin(), result.end()), result.end() );
        return result;
    }
};
```

