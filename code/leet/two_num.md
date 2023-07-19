> 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
>
> 你可以按任意顺序返回答案。
>
> 输入：nums = [2,7,11,15], target = 9
> 输出：[0,1]
> 解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 

[自己想法](#自己想法)
[查看解析](#查看解析)
[总结](#总结)

### 自己想法

执行用时：164 ms, 在所有 C++ 提交中击败了38.55% 的用户

内存消耗：9.9 MB, 在所有 C++ 提交中击败了88.36% 的用户

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>::iterator it_1,it_2;
        vector<int> order;
        int num1=0,num2=0;

        for(it_1=nums.begin();it_1!=nums.end();it_1++,num1++){
            num2=num1+1;

            for(it_2=nums.begin()+num1+1;it_2!=nums.end();it_2++,num2++){
                if(*it_1+*it_2==target&&it_1!=it_2){
                    order.push_back(num1);
                    order.push_back(num2);
                    return order;
                }
            }
        }
        return order;
    }
};
```

直接想法：

第一层正常遍历，第二层遍历，有相加等于target的就存入位置标记num1和num2

但是由于第一层遍历时，1和2比较过已经不匹配，所以在后续时，2不用再和1匹配，所以num2=num1+1，直接从第一层遍历元素的下一个开始进行下一次匹配

这目前是我的第一想法，时间复杂度也就比n方稍微好了一点点



### 查看解析：

1.用size控制，直接记录位置信息

但是我提交之后，运行时间会更长

> 执行用时：372 ms, 在所有 C++ 提交中击败了11.89% 的用户
>
> 内存消耗：9.9 MB, 在所有 C++ 提交中击败了83.63% 的用户

```c++
vector<int> twoSum(vector<int>& nums, int target){
	for (int i = 0; i < nums.size(); i++){
		for (int j = i + 1; j < nums.size(); j++){
			if (nums[i] + nums[j] == target){
				return{ i,j };
			}
		}
	}
	return {};
}
```

2.map，现在还没学到

> 拿map<key,value>举例，find()方法返回值是一个迭代器，成功返回迭代器指向要查找的元素，失败返回的迭代器指向end。
>
> count()方法返回值是一个整数，1表示有这个元素，0表示没有这个元素。

> 执行用时: **12 ms**
>
> 内存消耗: **11.9 MB**

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int,int> a;//建立hash表存放数组元素
        vector<int> b(2,-1);//存放结果
        
        for(int i=0;i<nums.size();i++)
            a.insert(map<int,int>::value_type(nums[i],i));	//把数据和位置存进去
        
        for(int i=0;i<nums.size();i++)
        {
            if(a.count(target-nums[i])>0&&(a[target-nums[i]]!=i))
            //count>0，代表存在target-当前元素，并且所在位置不是本身
            {
                b[0]=i;		//当前元素的位置
                b[1]=a[target-nums[i]];		//另一半的位置，map(键)=值
                break;		
            }
        }
        return b;
    };
};
```

3.进而改进

> 执行用时：4 ms, 在所有 C++ 提交中击败了99.26% 的用户
>
> 内存消耗：10.8 MB, 在所有 C++ 提交中击败了15.00% 的用户

一边插入的时候，就一边找之前插入的是否有另一半

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int,int> m1;
        vector<int> v1(2,-1);

        for(int i=0;i<nums.size();i++){
            m1.insert(map<int,int>::value_type(nums[i], i));    //顺序存入元素和位置

            //一边存入，一边找之前的是否是另一半？？
            if(m1.count(target-nums[i]) >0 && (m1[target-nums[i]]!=i)){
                v1[0]=i;
                v1[1]=m1[target-nums[i]];
                break;
            }
        }
        return v1;
    }
};
```

## 总结

1. 在一些数中找一个数，map非常适合
2. count()与find()区别
3. map比较，我们通过O(1)的比较，就获取了O(n)的信息，相较于num1+num2 ?=target，只是O(1)时间去获得O(1)的信息