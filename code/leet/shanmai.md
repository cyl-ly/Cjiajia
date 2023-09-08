#### [有效的山脉数组](https://leetcode.cn/problems/valid-mountain-array/description/)

> 给定一个整数数组 `arr`，如果它是有效的山脉数组就返回 `true`，否则返回 `false`。
>
> 让我们回顾一下，如果 `arr` 满足下述条件，那么它是一个山脉数组：
>
> - `arr.length >= 3`
> - 在 `0 < i < arr.length - 1`条件下，存在 `i`使得： 
>   - `arr[0] < arr[1] < ... arr[i-1] < arr[i] `
>   - `arr[i] > arr[i+1] > ... > arr[arr.length - 1]`

> ```
> 输入：arr = [2,1]
> 输出：false
> ```
>
> ```
> 输入：arr = [3,5,5]
> 输出：false
> ```
>
> ```
> 输入：arr = [0,3,2,1]
> 输出：true
> ```



#### 自己想法

1. 找到最大值，进行拆分，分别出来两段
2. 两段分别利用set去重，长度变短直接pass
3. set默认升序，和原数组比较，一直相同就是true

> 时间92ms，击败7%
>
> 内存34MB，击败5%

```c++
class Solution {
public:
    bool validMountainArray(vector<int>& arr) {
        if(arr.size()<3)
            return false;

        auto max=max_element(arr.begin(),arr.end());     //最大值迭代器
        if(*max==arr[0]||*max==arr[arr.size()-1])           //单调
            return false;
        
        set<int> cnt(arr.begin(),max);   //set会去重，而且默认升序排序
        set<int> cnt2(max,arr.end());   //后半段取出来，set特性可以去重

        if(cnt.size()+cnt2.size()!=arr.size())      //分别半段有重复的元素
            return false;

        vector<int> v1;
        v1.assign(cnt.begin(),cnt.end());   //整理好的复制回来

        for(int i=0;i<v1.size();i++){
            if(v1[i]!=arr[i])               //又不相等的就是false
                return false;
        }
        
        vector<int> v2;
        v2.assign(cnt2.begin(),cnt2.end());
        sort(v2.begin(),v2.end());    //降序排序
        reverse(arr.begin(),arr.end()); //翻转原序列

        for(int i=0;i<v2.size();i++){
            if(v2[i]!=arr[i])
                return false;
        }
        return true;
    }
};
```



#### 查看题解

1. 从前往后遍历，一直严格递增，然后一直严格递减
2. 要注意不能一直增，或者一直减

```c++
int i=0;
while(i+1<arr.size()&&arr[i]<arr[i+1])  i++;

if(i==0||i==arr.size()-1)
	return false;
while(i+1<arr.size()&&arr[i]>arr[i+1]) i++;

return i==arr.size()-1;
```

```c++
class Solution {
public:
    bool validMountainArray(vector<int>& arr) {
        if(arr.size()<3)
            return false;

        bool flag=true;

        auto max=max_element(arr.begin(),arr.end());
        int index=max-arr.begin();
        
        if(index==0||index==arr.size()-1)
            return false;

        for(int i=0;i<index;i++){
            if(arr[i]>=arr[i+1])    //出现非递增
                flag=false;
        }
        for(int i=arr.size()-1;i>index;i--){
            if(arr[i]>=arr[i-1])
                flag=false;
        }

        return flag;
    }
};
```



#### 总结

1. 不能用`vector.end()--`来返回最后一个元素迭代器

2. `reverse()`函数可以翻转顺序

3. set容器默认去重，且升序排序，可以通过复制vector进行排序去重

   ```c++
   set<int> s1(cnt.begin(), cnt.end());
   vector<int> v1(s1.begin(),s1.end());
   ```

4. `vector`排序函数`sort(begin,end);`

5. `vector`去重`v1.erase(unique(v1.begin(),v1.end()), v1.end());`

6. 最大值最小值函数`max_element()`，返回的是迭代器

7. 利用`max_element()`之后可以`-begin()`求得下标呀