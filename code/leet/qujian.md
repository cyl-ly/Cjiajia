#### [无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/description/)

> 给定一个区间的集合 `intervals` ，其中 `intervals[i] = [starti, endi]` 。返回 *需要移除区间的最小数量，使剩余区间互不重叠* 。

> ```
> 输入: intervals = [[1,2],[2,3],[3,4],[1,3]]
> 输出: 1
> 解释: 移除 [1,3] 后，剩下的区间没有重叠。
> ```
>
> ```
> 输入: intervals = [ [1,2], [1,2], [1,2] ]
> 输出: 2
> 解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
> ```
>
> ```
> 输入: intervals = [ [1,2], [2,3] ]
> 输出: 0
> 解释: 你不需要移除任何区间，因为它们已经是无重叠的了。
> ```



#### 自己想法

1. 贪心算法的区间问题
2. 计数重叠区间的层数不适用，有可能会漏层
3. 判断要重叠还是不要重叠，要不要修改右边界

```c++
bool cmp(const vector<int>& a, const vector<int>& b){
    return a[0]<b[0];
}

class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);

        int count=0;        //不重叠的区间数量

        for(int i=1;i<intervals.size();i++){
            if(intervals[i][0] >= intervals[i-1][1]){   //无重叠
                
            }else{
                count++;
                intervals[i][1]=min(intervals[i][1],intervals[i-1][1]);
            }
        }
        return count;
    }
};
```



#### 总结

1. 贪心算法的区间问题
2. cmp排序函数