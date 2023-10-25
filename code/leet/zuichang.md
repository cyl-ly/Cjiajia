## [最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

> 给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
>
> 请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

> ```
> 输入：nums = [100,4,200,1,3,2]
> 输出：4
> 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
> ```
>
> ```
> 输入：nums = [0,3,7,2,5,8,4,6,0,1]
> 输出：9
> ```

#### 题解

1. 很有意思，先复制，再去小循环找局部

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int length = 0;
        unordered_set<int> st(nums.begin(), nums.end());
        
        for(auto i:st){
            int res = 1;
            if(!st.count(i-1)){        //理解错了，不是每个数都会去找后续的数，而是当前的数是一个起点的时候
                while(st.count(++i))    //小循环去找他后面的所有数
                    res++;
            }
            length = max(length, res);
        }
        return length;
    }
};
```

## 