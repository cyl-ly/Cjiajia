#### [二分查找](https://leetcode.cn/problems/binary-search/)

1. 先看查找的区间是`[)`，还是`[]`，分不同情况进行判断
2. []

```c++
left = 0;
right = length-1;
while(left <= right){		//[1,1]这种情况，是需要等于的
	middle = (left + right)/2;		//int相加有可能越界
	if(nums[middle] > target)
		right = middle-1;		//middle的值已经不是我们想要的了，就等于-1
	else if(nums[middle < target])
		left = middle+1;
	else return middle;
}
return -1;
```

3. [)

```c++
left = 0;
right = length-1;
while(left < right){		//[1,1)这种情况，是需要等于的
	middle = (left + right)/2;		//int相加有可能越界
	if(nums[middle] > target)
		right = middle;		//middle的值已经不是我们想要的了，但右边是开区间
	else if(nums[middle < target])
		left = middle+1;	//middle不是想要的，左侧是闭区间，middle+1
	else return middle;
}
return -1;
```

> 给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target`  ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`

> ```
> 输入: nums = [-1,0,3,5,9,12], target = 9
> 输出: 4
> 解释: 9 出现在 nums 中并且下标为 4
> ```
>
> ```
> 输入: nums = [-1,0,3,5,9,12], target = 2
> 输出: -1
> 解释: 2 不存在 nums 中因此返回 -1
> ```



#### 自己想法

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;
        int middle;
        while(left<=right){
            middle = (left+right)/2;
            if(nums[middle] > target)
                right = middle-1;
            else if(nums[middle] < target)
                left = middle+1;
            else return middle;
        }
        return -1;
    }
};
```

