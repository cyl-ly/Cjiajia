#### [买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/description/)

> 给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。
>
> 你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。
>
> 返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

> ```
> 输入：[7,1,5,3,6,4]
> 输出：5
> 解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
>      注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
> ```
>
> ```
> 输入：prices = [7,6,4,3,1]
> 输出：0
> 解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
> ```



#### 自己想法

1. 动态规划
2. `dp`数组为当前日期的利润` == 当前价格 - 此前时间的min价格`
3. 一次遍历，用`min`不断更新已经遍历过的最低价格
4. `max`记录最大的`dp[i]`

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        //dp[i]的含义，就是此时卖出产生的利润
        //dp数组的迭代方式，就是当前的价格-前面最小的价格==最大的利润
        //dp[i]=dp[i-1]
        //从前向后进行遍历，并且初始化dp[0]=0
        vector<int> dp(n);    
        dp.push_back(0);
        vector<int>::iterator it;
        
        int max=0;
        int min=prices[0];      //最小值初始化为prices[0]
        for(int i=1;i<n;i++){
            dp[i]=prices[i]-min;
            if(dp[i] > max)
                max=dp[i];
            if(prices[i] < min)
                min=prices[i];
        }
        
        return max;
    }
};
```



#### 总结

1. 搞清晰dp数组的含义，以及最后要求的是什么，能够一次遍历结束的，都可以附加在条件里面
2. 以及`dp[i]`的迭代计算方式，并不一定就是要和前面的`dp[i-1]`和`dp[i-2]`有关，不要产生误区