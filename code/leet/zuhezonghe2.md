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