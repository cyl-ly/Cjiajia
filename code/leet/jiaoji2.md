## [两个数组的交集2](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)

> 给你两个整数数组 `nums1` 和 `nums2` ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序

> ```
> 输入：nums1 = [1,2,2,1], nums2 = [2,2]
> 输出：[2,2]
> ```
>
> ```
> 输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
> 输出：[4,9]
> ```

> - `1 <= nums1.length, nums2.length <= 1000`
> - `0 <= nums1[i], nums2[i] <= 1000`



#### 自己想法

1. 需要保留数字出现的次数，所以不能直接用set，里面只有关键字，没有key
2. 所以用map来进行保留，但是直接用num数组进行去查找是否重复，会重复，可以利用set的去重特性去进行查找

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        //unordered_set复制vector可以直接去重
        //利用auto遍历set的操作
        
        map<int,int> cnt1;
        map<int,int> cnt2;
        vector<int> p;      //存放结果
        for(int i=0; i<nums1.size(); i++){      //两个数组里面出现次数
            cnt1[nums1[i]]++;
        }
        for(int i=0; i<nums2.size(); i++){
            cnt2[nums2[i]]++;
        }

        unordered_set<int> s(nums2.begin(), nums2.end());
        for(auto i : s){
            if(cnt1.find(i) != cnt1.end()){
                int num = min(cnt1[i], cnt2[i]);
                while(num--)
                    p.push_back(i);
            }
        }
        return p;

    }
};
```

