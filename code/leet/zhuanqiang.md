## [砖墙](https://leetcode.cn/problems/brick-wall/submissions/477302341/)

> 你的面前有一堵矩形的、由 `n` 行砖块组成的砖墙。这些砖块高度相同（也就是一个单位高）但是宽度不同。每一行砖块的宽度之和相等。
>
> 你现在要画一条 **自顶向下** 的、穿过 **最少** 砖块的垂线。如果你画的线只是从砖块的边缘经过，就不算穿过这块砖。**你不能沿着墙的两个垂直边缘之一画线，这样显然是没有穿过一块砖的。**
>
> 给你一个二维数组 `wall` ，该数组包含这堵墙的相关信息。其中，`wall[i]` 是一个代表从左至右每块砖的宽度的数组。你需要找出怎样画才能使这条线 **穿过的砖块数量最少** ，并且返回 **穿过的砖块数量** 。

> ```
> 输入：wall = [[1,2,2,1],[3,1,2],[1,3,2],[2,4],[3,1,2],[1,3,1,1]]
> 输出：2
> ```
>
> ```
> 输入：wall = [[1],[1],[1]]
> 输出：3
> ```

#### 题解

1. 用针打气球，一样又不一样，那个会错位，需要动态规划

```c++
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<int, int> mp(wall[0].size());
        int result = 0;
        for(int i=0; i<wall.size(); i++){
            int sum = 0;
            for(int j=0; j<wall[i].size()-1; j++){  //哪里有边界，记录哪里，但是最后一块不记录
                sum += wall[i][j];
                mp[sum]++;
            }
        }
        for(auto i:mp)
            result = max(result, i.second);
        return wall.size()-result;
    }
};
```

