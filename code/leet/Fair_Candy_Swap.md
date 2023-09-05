#### [公平的糖果交换](https://leetcode.cn/problems/fair-candy-swap/description/)

##### 题目描述

> 爱丽丝和鲍勃拥有不同总数量的糖果。给你两个数组 `aliceSizes` 和 `bobSizes` ，`aliceSizes[i]` 是爱丽丝拥有的第 `i` 盒糖果中的糖果数量，`bobSizes[j]` 是鲍勃拥有的第 `j` 盒糖果中的糖果数量。
>
> 两人想要互相交换一盒糖果，这样在交换之后，他们就可以拥有相同总数量的糖果。一个人拥有的糖果总数量是他们每盒糖果数量的总和。
>
> 返回一个整数数组 `answer`，其中 `answer[0]` 是爱丽丝必须交换的糖果盒中的糖果的数目，`answer[1]` 是鲍勃必须交换的糖果盒中的糖果的数目。如果存在多个答案，你可以返回其中 **任何一个** 。题目测试用例保证存在与输入对应的答案。

##### 测试案例

> ```
> 输入：aliceSizes = [1,1], bobSizes = [2,2]
> 输出：[1,2]
> ```
>
> ```
> 输入：aliceSizes = [1,2], bobSizes = [2,3]
> 输出：[1,2]
> ```
>
> ```
> 输入：aliceSizes = [2], bobSizes = [1,3]
> 输出：[2,3]
> ```
>
> ```
> 输入：aliceSizes = [1,2,5], bobSizes = [2,4]
> 输出：[5,4]
> ```



#### 自己想法

1. 先计算每个集合的和，进而找到差值，每个集合增加或减少一半差值就可以达到交换后相等的效果

> 时间456ms，击败9%
>
> 内存38MB，击败87%

```c++
class Solution {
public:
    vector<int> fairCandySwap(vector<int>& aliceSizes, vector<int>& bobSizes) {
        int sum1=accumulate(aliceSizes.begin(), aliceSizes.end(), 0);
        int sum2=accumulate(bobSizes.begin(), bobSizes.end(), 0);

        int num=(sum1-sum2)/2;
            
        for(int i=0; i<aliceSizes.size(); i++){
            vector<int>::iterator it=find(bobSizes.begin(), bobSizes.end(), aliceSizes[i]-num);
            if(it!=bobSizes.end()){
                return {aliceSizes[i], aliceSizes[i]-num};
            }
        }
        return {0,0};
    }
};
```



#### 查看题解

1. 我在查找元素的时候，是在`vector`中用迭代器进行查找，但发现将其复制到无序（哈希）容器里面，查找好像会更快一点

```c++
class Solution {
public:
    vector<int> fairCandySwap(vector<int>& aliceSizes, vector<int>& bobSizes) {
        int sum1=accumulate(aliceSizes.begin(), aliceSizes.end(), 0);
        int sum2=accumulate(bobSizes.begin(), bobSizes.end(), 0);

        int num=(sum1-sum2)/2;
            
        unordered_set<int> copy(bobSizes.begin(),bobSizes.end());

        for(auto& i:aliceSizes){
            if(copy.count(i-num))
                return {i,i-num};
        }
        return {0,0};
    }
};
```



#### 总结

1. `vector`的`accumulate`函数
2. 哈希容器，`unorded_set<int>`
3. `vector`中`find`查找是返回迭代器，比`count`速度要快一点