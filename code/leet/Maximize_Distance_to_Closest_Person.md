#### [到最近人的最远距离](https://leetcode.cn/problems/maximize-distance-to-closest-person/description/)

> 给你一个数组 `seats` 表示一排座位，其中 `seats[i] = 1` 代表有人坐在第 `i` 个座位上，`seats[i] = 0` 代表座位 `i` 上是空的（**下标从 0 开始**）。
>
> 至少有一个空座位，且至少有一人已经坐在座位上。
>
> 亚历克斯希望坐在一个能够使他与离他最近的人之间的距离达到最大化的座位上。
>
> 返回他到离他最近的人的最大距离。

> ```
> 输入：seats = [1,0,0,0,1,0,1]
> 输出：2
> 解释：
> 如果亚历克斯坐在第二个空位（seats[2]）上，他到离他最近的人的距离为 2 。
> 如果亚历克斯坐在其它任何一个空位上，他到离他最近的人的距离为 1 。
> 因此，他到离他最近的人的最大距离是 2
> ```

> ```
> 输入：seats = [1,0,0,0]
> 输出：3
> 解释：
> 如果亚历克斯坐在最后一个座位上，他离最近的人有 3 个座位远。
> 这是可能的最大距离，所以答案是 3
> ```

> ```
> 输入：seats = [0,1]
> 输出：1
> ```



#### 我的想法

> 时间16ms，击败30%
>
> 内存17MB，击败6%

```c++
class Solution {
public:
    int maxDistToClosest(vector<int>& seats) {
        vector<pair<int,int>> v1;

        for(int i=0;i<seats.size();i++){

            if(seats[i]==1){
                v1.push_back({i,i});       //v1的下标和在seats中的下标
             //   mm++;
            }
        }

        int max=0;
        int an=0;
        for(int i=0,j=1;j<v1.size();i++,j++){
            int num=v1[j].first-v1[i].first;

            if(num>max){
                max=num;
                an=(v1[j].first+v1[i].first)/2-v1[i].first;

            }
        }

        if(seats[0]==0){
            int num=v1[0].first;
            if(num>=an){
                an=num;
            }
        }

        if(seats[seats.size()-1]==0){
            int num=seats.size()-v1[v1.size()-1].first-1;
            cout<<num<<" "<<max<<endl;
            if(num>=an){
                an=num;
            }
        }
        return an;
    }
};
```

1. 先把所有有人的位置找出来，存放在vector<pair<int,int>>中，分为三种情况
2. 中间段（两边有人，中间空），找到最大段，然后记录位置就可以，需要注意的是，这里记录的距离是最大段中离得较近的那个人的距离
3. 头尾段（只有一侧有人），直接在pair中找到第一（末1）有人的距离，然后计算前面（后面）有多少空座就可以，需要注意的是，这里是需要和上面已经找到的较近的距离比较，而不是和最大段进行比较



#### 查看题解

1. 用first和last记录第一个和最后一个有人的位置
2. 从头编历时，每次遇到人都是最后一个，所以可以同时记录中间遇到的最大段

```c++
class Solution {
public:
    int maxDistToClosest(vector<int>& seats) {
        int first=-1,last=-1;
        int d=0;

        for(int i=0;i<seats.size();i++){
            if(seats[i]==1){
                if(first==-1){  //找到第一个
                    first=i;
                }
                if(last!=-1){
                    d=max(d, i-last);
                }
                last=i;
            }
        }

        int n=seats.size();
        first=max(first, n-last-1);
        return max(d/2,first);
    }
};
```

####  总结

1. vector<pair<int,int>>
2. 从头遍历，找到最大连续的段，用于后续的各种处理

