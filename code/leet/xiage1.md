## [下一个更大的元素I](https://leetcode.cn/problems/next-greater-element-i/description/)

> `nums1` 中数字 `x` 的 **下一个更大元素** 是指 `x` 在 `nums2` 中对应位置 **右侧** 的 **第一个** 比 `x` 大的元素。
>
> 给你两个 **没有重复元素** 的数组 `nums1` 和 `nums2` ，下标从 **0** 开始计数，其中`nums1` 是 `nums2` 的子集。
>
> 对于每个 `0 <= i < nums1.length` ，找出满足 `nums1[i] == nums2[j]` 的下标 `j` ，并且在 `nums2` 确定 `nums2[j]` 的 **下一个更大元素** 。如果不存在下一个更大元素，那么本次查询的答案是 `-1` 。
>
> 返回一个长度为 `nums1.length` 的数组 `ans` 作为答案，满足 `ans[i]` 是如上所述的 **下一个更大元素** 。

> ```
> 输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
> 输出：[-1,3,-1]
> 解释：nums1 中每个值的下一个更大元素如下所述：
> - 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
> - 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
> - 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
> ```
>
> ```
> 输入：nums1 = [2,4], nums2 = [1,2,3,4].
> 输出：[3,-1]
> 解释：nums1 中每个值的下一个更大元素如下所述：
> - 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
> - 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
> ```

#### 题解

1. 左神个人号，单调栈上

```c++
class Solution {
private:
    int stack[1001];    //模拟栈
    int cur = 0;        //栈内元素
    int result[1001];   //保存下一个更大的元素，保存下标
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        //先求出nums2的所有下一个更大元素
        //遍历阶段
        for(int i=0; i<nums2.size(); i++){
            while(cur > 0 && nums2[stack[cur-1]] <= nums2[i]){     //当前的更大，把里面的顶出来
                int index = stack[--cur];   //取出栈顶下标
                result[index] = i;  //让你出栈的，就是比你大的
            }
            stack[cur++] = i;   //放入元素
        }
        //清算阶段
        while(cur > 0){
            int index = stack[--cur];
            result[index] = -1;
        }
        //题目告诉了，没有重复的元素，否则还需要加上检查阶段
        //检查阶段
        // for(int i=nums2.size()-2; i>=0; i--){   //最后一个一定结果是-1
        //     if(result[i] != -1 && nums2[result[i]] == nums2[i])
        //         result[i] = result[result[i]];      //把下一个数的result进行保存
        // }

        vector<int> cnt(nums1.size());    //题目的结果保存
        for(int i=0; i<nums1.size(); i++){
            int index = find(nums2.begin(), nums2.end(), nums1[i])-nums2.begin();
            cnt[i] = result[index] == -1 ? -1 : nums2[result[index]];
        }
        return cnt;
    }
};
```

