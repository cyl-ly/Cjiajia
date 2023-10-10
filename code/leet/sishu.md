## [四数相加2](https://leetcode.cn/problems/4sum-ii/)

> 给你四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` ，数组长度都是 `n` ，请你计算有多少个元组 `(i, j, k, l)` 能满足：
>
> - `0 <= i, j, k, l < n`
> - `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

> ```
> 输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
> 输出：2
> 解释：
> 两个元组如下：
> 1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
> 2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
> ```
>
> ```
> 输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
> 输出：1
> ```



#### 自己想法

1. 最后输出是和次数相关的，就可以用map进行存储
2. 把四数之和，转变为两数之和，时间复杂度变为n方

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        //把两个数组看做一组，转为“两数之和”
        //把a+b和次数，进行一个存储
        int result = 0;     //结果
        map<int,int> cnt1;
        for(int i=0; i<nums1.size(); i++){
            for(int j=0; j<nums1.size(); j++)
                cnt1[nums1[i] + nums2[j]]++;        //a+b的统计完毕了,key是次数
        }
        for(int i=0; i<nums3.size(); i++){
            for(int j=0; j<nums4.size(); j++){
                int num = nums3[i]+nums4[j];
                int want = -num;        //要找的数字
                if(cnt1.find(want) != cnt1.end()){//找到了
                    result+=cnt1[want];
                }
            }   
        }
        return result;
    }
};
```

