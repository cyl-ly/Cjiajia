## [山脉数组的峰顶索引](https://leetcode.cn/problems/peak-index-in-a-mountain-array/)

> 符合下列属性的数组 `arr` 称为 **山脉数组** ：
>
> - `arr.length >= 3`
> - 存在 i（0 < i < arr.length - 1）使得： 
>   - `arr[0] < arr[1] < ... arr[i-1] < arr[i] `
>   - `arr[i] > arr[i+1] > ... > arr[arr.length - 1]`
>
> 给你由整数组成的山脉数组 `arr` ，返回满足 `arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]` 的下标 `i` 。
>
> 你必须设计并实现时间复杂度为 `O(log(n))` 的解决方案。

> ```
> 输入：arr = [0,1,0]
> 输出：1
> ```
>
> ```
> 输入：arr = [0,2,1,0]
> 输出：1
> ```
>
> ```
> 输入：arr = [0,10,5,2]
> 输出：1
> ```



#### 自己想法

1. 二分法的应用——局部最小（大）值

```c++
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        //已知长度大于等于3，最先可能存在的点是1和n-2，先判断
        //right更新的时候要=mid，因为mid-1很可能会是答案，如果等于mid-1的话，还会去左边寻找
        //具体图解二分法笔记上有

        if(arr[1] > arr[0] && arr[1] > arr[2])
            return 1;
        int n = arr.size();
        if(arr[n-2] > arr[n-1] && arr[n-2] > arr[n-3])
            return n-2;

        int left = 0, right = n-1;  //下标
        while(left <= right){
            int mid = (left+right)/2;
            if(arr[mid] < arr[mid-1])
                right = mid;
            else if(arr[mid] < arr[mid+1])
                left = mid;
            else if(arr[mid] >arr[mid-1] && arr[mid] > arr[mid+1])
                return mid;
        }
        return 0;
    }
};
```

