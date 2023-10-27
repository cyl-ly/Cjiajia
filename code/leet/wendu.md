## [每日温度](https://leetcode.cn/problems/daily-temperatures/)

> 给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

> ```
> 输入: temperatures = [73,74,75,71,69,72,76,73]
> 输出: [1,1,4,2,1,1,0,0]
> ```
>
> ```
> 输入: temperatures = [30,40,50,60]
> 输出: [1,1,1,0]
> ```
>
> ```
> 输入: temperatures = [30,60,90]
> 输出: [1,1,0]
> ```

#### 题解

```c++
class Solution {
public:
    int stack[100001];      //模拟栈
    int cur = 0;
    int result[100001];     //单调栈下标结果
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        
        //遍历阶段
        int n = temperatures.size();
        for(int i=0; i<n; i++){
            while(cur > 0 && temperatures[stack[cur-1]] < temperatures[i]){
                int index = stack[--cur];
                result[index] = i;
            }
            stack[cur++] = i;
        }
        //清算阶段
        while(cur > 0){
            int index = stack[--cur];
            result[index] = -1;
        }

        //好像不需要检查阶段，用<号严格控制了
        //保存本题结果
        vector<int> cnt(n);
        for(int i=0; i<n; i++){
            cnt[i] = result[i] == -1 ? 0 : result[i]-i;
        }
        return cnt;
    }
};
```

