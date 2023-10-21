## [交易逆序对的总数](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/submissions/475721928/)

> 在股票交易中，如果前一天的股价高于后一天的股价，则可以认为存在一个「交易逆序对」。请设计一个程序，输入一段时间内的股票交易记录 `record`，返回其中存在的「交易逆序对」总数。

> ```
> 输入：record = [9, 7, 5, 4, 6]
> 输出：8
> 解释：交易中的逆序对为 (9, 7), (9, 5), (9, 4), (9, 6), (7, 5), (7, 4), (7, 6), (5, 4)。
> ```



#### 自己想法

1. 左神归并排序的扩展应用——逆序对

```c++
class Solution {
public:
    int sum = 0;
    void merge_nixu(vector<int> &nums, int left, int mid, int right){
	    vector<int> help(right-left+1, 0);
	    int p1 = left;
	    int p2 = mid+1;
	    int i = 0;		//help下标
	    while(p1 <= mid && p2 <=right){
		    sum += nums[p1] > nums[p2] ? (mid-p1+1) : 0;
		    help[i++] = nums[p1] <= nums[p2] ? nums[p1++] : nums[p2++];
		//这里要注意，左右侧==时，要先拷贝左侧的，然后笔记有图示
	    }
	    while(p1 <= mid){
		    help[i++] = nums[p1++];		//剩余的部分
	    }
	    while(p2 <= right){
		    help[i++] = nums[p2++];
	    }
	    for(i=0; i<help.size(); i++)
		    nums[left+i] = help[i];
	
	    vector<int>().swap(help);	//释放内存空间
    }
    void guibing_sort(vector<int> &nums, int left, int right){
	    if(left == right)	//一个数字
	    	return;
	    int mid = left+(right-left)/2;
	    guibing_sort(nums, left, mid);
	    guibing_sort(nums, mid+1, right);
	    //merge(nums, left, mid, right);		//归并
	    //merge_xiaohe(nums, left, mid, right);		//小和问题
	    merge_nixu(nums, left, mid, right);		//逆序对
    }
    int reversePairs(vector<int>& record) {
			if(record.size() <= 1)
				return 0;
			
      guibing_sort(record, 0, record.size()-1);
      return sum;
    }
};
```

