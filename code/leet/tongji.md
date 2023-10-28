## [统计全1子矩形](https://leetcode.cn/problems/count-submatrices-with-all-ones/description/)

> 给你一个 `m x n` 的二进制矩阵 `mat` ，请你返回有多少个 **子矩形** 的元素全部都是 1 。

> ```
> 输入：mat = [[1,0,1],[1,1,0],[1,1,0]]
> 输出：13
> 解释：
> 有 6 个 1x1 的矩形。
> 有 2 个 1x2 的矩形。
> 有 3 个 2x1 的矩形。
> 有 1 个 2x2 的矩形。
> 有 1 个 3x1 的矩形。
> 矩形数目总共 = 6 + 2 + 3 + 1 + 1 = 13 。
> ```
>
> ```
> 输入：mat = [[0,1,1,0],[0,1,1,1],[1,1,1,0]]
> 输出：24
> 解释：
> 有 8 个 1x1 的子矩形。
> 有 5 个 1x2 的子矩形。
> 有 2 个 1x3 的子矩形。
> 有 4 个 2x1 的子矩形。
> 有 2 个 2x2 的子矩形。
> 有 2 个 3x1 的子矩形。
> 有 1 个 3x2 的子矩形。
> 矩形数目总共 = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24 。
> ```

#### 题解

```c++
class Solution {
public:
    int stack[151]; //模拟栈
    int cur = 0;
    //int result[151][2];     //左右更小的结果

    void compute(vector<int> &nums, int &cnt){    //每次进来一层
        //遍历阶段
        for(int i=0; i<nums.size(); i++){
            while(cur > 0 && nums[i] < nums[stack[cur-1]]){//相等也出去，后面会补
                int index = stack[--cur];   //出栈
                if(nums[index] > nums[i]){
                    //这里什么时候结算很重要，
                    //如果是因为相等弹出的，就先不计算，之后最后一个相等的会给补充上
                    int left = cur > 0 ? stack[cur-1] : -1;
                    //L  index  i
                    int high = max(left == -1 ? 0 : nums[left], nums[i]);
                    int len = i-left-1;
                    cnt += (nums[index]-high)*len*(len+1)/2;
                    //找到左右中较大的，计算与自己的高度差值，和宽度共同计算矩形个数
                }
            }
            stack[cur++] = i;
        }
        //清算阶段
        while(cur > 0){
            int index = stack[--cur];
            int left = cur > 0 ? stack[cur-1] : -1;
            int len = nums.size()-left-1;
            int high = left == -1 ? 0 : nums[left];
            cnt += (nums[index]-high)*len*(len+1)/2;
            //左右都不存在的时候，就按自己的高度进行计算
        }
    }
    int numSubmat(vector<vector<int>>& mat) {
        //压缩数组，每次进去一层，找到以每层为底的矩形的个数
        int cnt = 0;
        vector<int> count(mat[0].size());

        for(int i=0; i<mat.size(); i++){
            for(int j=0; j<mat[0].size(); j++){
                count[j] = mat[i][j] == 1 ? count[j]+1 : 0;
            }
            compute(count, cnt);
            cout<<cnt<<endl;
        }
        return cnt;
    }
};
```

