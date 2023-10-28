## [子数组的最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/description/)

> 给定一个整数数组 `arr`，找到 `min(b)` 的总和，其中 `b` 的范围为 `arr` 的每个（连续）子数组。
>
> 由于答案可能很大，因此 **返回答案模 `10^9 + 7`** 。

> ```
> 输入：arr = [3,1,2,4]
> 输出：17
> 解释：
> 子数组为 [3]，[1]，[2]，[4]，[3,1]，[1,2]，[2,4]，[3,1,2]，[1,2,4]，[3,1,2,4]。 
> 最小值为 3，1，2，4，1，1，2，1，1，1，和为 17。
> ```
>
> ```
> 输入：arr = [11,81,94,43,3]
> 输出：444
> ```

#### 题解

```c++
class Solution {
public:
    int stack[30001];
    int cur = 0;
    int result[30001][2];   //保存前面比他小的，和后面比他小的  下标
    int m = 1e9+7;
    void compute(vector<int> &arr, long long &sum){
        //遍历阶段
        for(int i=0; i<arr.size(); i++){
            while(cur > 0 && arr[stack[cur-1]] > arr[i]){
                int index = stack[--cur];
                result[index][1] = i;       //后面比他小的
                result[index][0] = cur > 0 ? stack[cur-1] : -1;    //前面比他小的，就是栈里面的邻居
                //result[index][0]   index   i
                //在左右范围内，且同时包含index的子数组，最小值一定是index，求出组数*arr[index]
                sum = (sum + (long long)(i-index)*(index-result[index][0])*arr[index]) % m; //弹出栈就结算一个
            }
            stack[cur++] = i;
        }
        //清算阶段
        while(cur > 0){
            int index = stack[--cur];
            result[index][1] = -1;
            result[index][0] = cur > 0 ? stack[cur-1] : -1;
            sum = (sum + (long long)(arr.size()-index)*(index-result[index][0])*arr[index]) % m;
        }
        //修正阶段
        // for(int i=arr.size()-2; i>=0; i--){
        //     if(result[i][1] != -1 && arr[result[i][1]] == arr[i])
        //         result[i][1] = result[result[i][1]];
        // }
    }
    int sumSubarrayMins(vector<int>& arr) {
        long long sum = 0;
        compute(arr, sum);   //result中保存的就是下标结果
        return sum;
    }
};
```

