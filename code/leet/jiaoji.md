## [两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

> 给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

> ```
> 输入：nums1 = [1,2,2,1], nums2 = [2,2]
> 输出：[2]
> ```
>
> ```
> 输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
> 输出：[9,4]
> 解释：[4,9] 也是可通过的
> ```



> - `1 <= nums1.length, nums2.length <= 1000`
> - `0 <= nums1[i], nums2[i] <= 1000`



#### 自己想法

1. 利用map去统计结果，但是需要根据set去重之后再返回结果

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        map<int, int> cnt;
        vector<int> p;  //存放结果
        for(int i=0; i<nums1.size(); i++){
            cnt[nums1[i]]++;        //先把num1里面的都找出来
        }
        for(int i=0; i<nums2.size(); i++){
            if(cnt.find(nums2[i]) != cnt.end()){
                p.push_back(nums2[i]);
            }
        }
        set<int> s(p.begin(), p.end());     //利用set的特性去重
        p.assign(s.begin(), s.end());
        return p;
    }
};
```

2. 利用数组实现哈希

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> cnt;     //存放结果
        int p[1000] = {0};  //利用数组实现哈希，进行初始化

        for(int i=0; i<nums1.size(); i++)
            p[nums1[i]]++;
        for(int i=0; i<nums2.size(); i++){
            if(p[nums2[i]] != 0){       //2中对应的元素在1中已经存在了
                cnt.insert(nums2[i]);
            }
        }
        return vector<int>(cnt.begin(), cnt.end());
    }
};
```

3. set实现哈希

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result;      //存放结果，利用set特性去重

        unordered_set<int> p(nums1.begin(), nums1.end());
        for(int i=0; i<nums2.size(); i++){
            if(p.find(nums2[i]) != p.end())     //找到了元素存在
                result.insert(nums2[i]);
        }
        return vector<int>(result.begin(), result.end());
    }
};
```

