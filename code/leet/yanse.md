## [颜色分类](https://leetcode.cn/problems/sort-colors/)

> 给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
>
> 我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。
>
> 必须在不使用库内置的 sort 函数的情况下解决这个问题。

> ```
> 输入：nums = [2,0,2,1,1,0]
> 输出：[0,0,1,1,2,2]
> ```
>
> ```
> 输入：nums = [2,0,1]
> 输出：[0,1,2]
> ```



#### 自己想法

1. 左神《荷兰国旗》变形

```c++
class Solution {
public:
    void helanII(vector<int> &nums, int key){
	int low_end = 0;	//小于区域的右边界+1
	int high_end = nums.size()-1;		//大于区域的左边界-1
	for(int i=0; i<=high_end; i++){	//结束条件很有趣F8 debug出来的
		if(nums[i] > key){	
			swap(nums[high_end--], nums[i--]);	//大于时i不动
		}
		else if(nums[i] < key){
			swap(nums[low_end++], nums[i]);
		}
		//等于时没有操作
	}
    }
    void sortColors(vector<int>& nums) {
        //荷兰国旗II变形
        helanII(nums, 1);
    }
};
```

