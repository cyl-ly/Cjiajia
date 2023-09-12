#### [等价多米诺骨牌对的数量](https://leetcode.cn/problems/number-of-equivalent-domino-pairs/description/)

> 给你一个由一些多米诺骨牌组成的列表 `dominoes`。
>
> 如果其中某一张多米诺骨牌可以通过旋转 `0` 度或 `180` 度得到另一张多米诺骨牌，我们就认为这两张牌是等价的。
>
> 形式上，`dominoes[i] = [a, b]` 和 `dominoes[j] = [c, d]` 等价的前提是 `a==c` 且 `b==d`，或是 `a==d` 且 `b==c`。
>
> 在 `0 <= i < j < dominoes.length` 的前提下，找出满足 `dominoes[i]` 和 `dominoes[j]` 等价的骨牌对 `(i, j)` 的数量。

> ```
> 输入：dominoes = [[1,2],[2,1],[3,4],[5,6]]
> 输出：1
> ```
>
> ```
> 输入：dominoes = [[1,2],[1,2],[1,1],[1,2],[2,2]]
> 输出：3
> ```



#### 自己想法

1. 暴力两层遍历，但是超时了

```c++
class Solution {
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        int n=dominoes.size();
        int num=0;
        for(int i=0;i<n-1;i++)
        {
            int x=dominoes[i][0];
            int y=dominoes[i][1];
            for(int j=i+1;j<n;j++)
            {
                int m=dominoes[j][0];
                int n=dominoes[j][1];
                if((x==m&&y==n)||(x==n&&y==m))
                        num++; 
            }
        }
        return num;
    }
};
```



#### 查看解析

1. 利用哈希表存储统计

```c++
class Solution {
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        map< vector<int>, int> mp;
        int sum=0;
        for(auto it: dominoes){
            if(it[1]<it[0]) swap(it[0],it[1]);  //小的在前
            mp[it]++;
        }
        for(auto it2:mp){
            if(it2.second>1)
                sum+=it2.second*(it2.second-1)/2;
        }
        return sum;
    }
};
```



1. 直接调转数字，变为一维问题，进行统计

```c++
class Solution {
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        int c[100]={0};
        int sum=0;
        for(auto& it :dominoes)
        {
            int num= it[0]>it[1] ? it[1]*10+it[0] : it[0]*10+it[1]; //找到对应的数字
            
            c[num]++;
            sum=sum+c[num]-1;
            
        }
        return sum;
    }
};
```



#### 总结

1. 不用判断map是否第一次出现，直接++，用数学公式统计次数

2. ```c++
   map<vector<int>,int> mp;
   for(auto it: dominoes){
       if(it[1]<it[0]) swap(it[0],it[1]);  //小的在前
           mp[it]++;
   }
   for(auto it2:mp){
       if(it2.second>1)
           sum+=it2.second*(it2.second-1)/2;
   }
   ```

