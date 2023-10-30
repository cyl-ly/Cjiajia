## [航班预定统计](https://leetcode.cn/problems/corporate-flight-bookings/submissions/478223263/)

> 这里有 `n` 个航班，它们分别从 `1` 到 `n` 进行编号。
>
> 有一份航班预订表 `bookings` ，表中第 `i` 条预订记录 `bookings[i] = [firsti, lasti, seatsi]` 意味着在从 `firsti` 到 `lasti` （**包含** `firsti` 和 `lasti` ）的 **每个航班** 上预订了 `seatsi` 个座位。
>
> 请你返回一个长度为 `n` 的数组 `answer`，里面的元素是每个航班预定的座位总数。

> ```
> 输入：bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
> 输出：[10,55,45,25,25]
> 解释：
> 航班编号        1   2   3   4   5
> 预订记录 1 ：   10  10
> 预订记录 2 ：       20  20
> 预订记录 3 ：       25  25  25  25
> 总座位数：      10  55  45  25  25
> 因此，answer = [10,55,45,25,25]
> ```
>
> ```
> 输入：bookings = [[1,2,10],[2,2,15]], n = 2
> 输出：[10,25]
> 解释：
> 航班编号        1   2
> 预订记录 1 ：   10  10
> 预订记录 2 ：       15
> 总座位数：      10  25
> 因此，answer = [10,25]
> ```

#### 题解

```c++
class Solution {
public:
    //差分数组，用前缀和进行累加
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        vector<int> result(n+2, 0);
        for(int i=0; i<bookings.size(); i++){
            result[bookings[i][0] - 1] += bookings[i][2];   //开头进行相加
            result[bookings[i][1]] -= bookings[i][2];   //末尾下一位进行相减
            //这里会超出长度，需要控制
        }
        for(int i=1; i<n; i++){
            result[i] += result[i-1];       //求前缀和
        }
        result.resize(n);   //把长度改为n，但是还比erase快
        return result;
    }
};
```

