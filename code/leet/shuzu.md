## [数组中的k-diff数组](https://leetcode.cn/problems/k-diff-pairs-in-an-array/)

> 给你一个整数数组 `nums` 和一个整数 `k`，请你在数组中找出 **不同的** k-diff 数对，并返回不同的 **k-diff 数对** 的数目。
>
> **k-diff** 数对定义为一个整数对 `(nums[i], nums[j])` ，并满足下述全部条件：
>
> - `0 <= i, j < nums.length`
> - `i != j`
> - `nums[i] - nums[j] == k`
>
> **注意**，`|val|` 表示 `val` 的绝对值。

> ```
> 输入：nums = [3, 1, 4, 1, 5], k = 2
> 输出：2
> 解释：数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
> 尽管数组中有两个 1 ，但我们只应返回不同的数对的数量。
> ```
>
> ```
> 输入：nums = [1, 2, 3, 4, 5], k = 1
> 输出：4
> 解释：数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5) 。
> ```
>
> ```
> 输入：nums = [1, 3, 1, 5, 4], k = 0
> 输出：1
> 解释：数组中只有一个 0-diff 数对，(1, 1) 。
> ```

#### 题解

1. 哈希表存储，加去重，时间ONlog，空间ON

```c++
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {
        //遍历，存哈希，找k-value
        sort(nums.begin(), nums.end());

        unordered_set<int> st;
        unordered_set<int> num;    //去重用的
        int result = 0;
        for(int i=0; i<nums.size(); i++){
            int n = nums[i]*10+nums[i]-k;       //做为一个组合存到哈希表里
            if(st.count(nums[i]-k) && !num.count(n)){           //怎么去重，1 1,k=0 和 2 2 3,k=1这样的去重
                result++;
                num.insert(n);
            }
            st.insert(nums[i]);
        }
        return result;
    }
};
```

## 