## [前k个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

> 给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

> ```
> 输入: nums = [1,1,1,2,2,3], k = 2
> 输出: [1,2]
> ```
>
> ```
> 输入: nums = [1], k = 1
> 输出: [1]
> ```



#### 自己想法

1. 自定义map比较函数，来不断找到map中的最大值

```c++
class Solution {
public:
    static bool cmp_value(const pair<int,int> a, const pair<int,int> b){
        return a.second<b.second;
    }
    //自定义map比较函数，在类内需要声明为static

    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int, int> cnt;
        vector<int> result(k);
        int index = 0;
        for(auto i:nums)
            cnt[i]++;       //下标与出现次数
        while(index != k){
            auto it = max_element(cnt.begin(), cnt.end(), cmp_value);
            result[index++] = it->first;
            it->second = 0;     //修改value，不再参与比较了
        }
        return result;
    }
};
```

2. 小根堆，pop的都是小元素，留下的都是大元素

```c++
class Solution {
public:
    struct cmp{
    // 小根堆
	    bool operator ()(const pair<int, int>& a, const pair<int, int>& b){
		return  a.second >  b.second;  
	    }
    };

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        for(auto i:nums)    cnt[i]++;       //下标与出现次数

        priority_queue<pair<int,int>, vector<pair<int,int>>, cmp > small_heap;   //小根堆
        for(auto it=cnt.begin(); it!=cnt.end(); it++){  
            //小根堆遍历，pop出去的都是小的，留下的都是大的元素
            small_heap.push(*it);
            if(small_heap.size() > k)
                small_heap.pop();
        }
        vector<int> result(k);
        for (int i = k - 1; i >= 0; i--) {
            result[i] = small_heap.top().first;
            small_heap.pop();
        }
        return result;
    }
};
```

