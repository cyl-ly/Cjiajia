## [洛谷-等差差分例题](https://www.luogu.com.cn/problem/P4231)

> *N*个柱子排成一排，一开始每个柱子损伤度为0。
>
> 接下来勇仪会进行�*M*次攻击，每次攻击可以用4个参数�*l*,�*r*,�*s*,�*e*来描述：
>
> 表示这次攻击作用范围为第�*l*个到第�*r*个之间所有的柱子(包含�*l*,�*r*)，对第一个柱子的伤害为�*s*，对最后一个柱子的伤害为�*e*。
>
> 攻击产生的伤害值是一个等差数列。若�=1*l*=1,�=5*r*=5,�=2*s*=2,�=10*e*=10，则对第1~5个柱子分别产生2,4,6,8,10的伤害。
>
> 鬼族们需要的是所有攻击完成之后每个柱子的损伤度。
>
> ## 输入格式
>
> 第一行2个整数�*N*,�*M*，用空格隔开，下同。
>
> 接下来�*M*行，每行4个整数�*l*,�*r*,�*s*,�*e*，含义见题目描述。
>
> 数据保证对每个柱子产生的每次伤害值都是整数。
>
> ## 输出格式
>
> 由于输出数据可能过大无法全部输出，为了确保你真的能维护所有柱子的损伤度，只要输出它们的异或和与最大值即可。
>
> （异或和就是所有数字按位异或起来的值）
>
> （异或运算符在c++里为^）

> ```
> 5 2
> 1 5 2 10
> 2 4 1 1
> 
> 3 10	//输出
> ```

#### 题解

```c++
#include<iostream>
using namespace std;

long long nums[10000005];		//数据
long long len;	//数组总长度

//int data1[300001][5];		//存储l r s e d	
void set(int l, int r, int s, int e, int d){
	nums[l] += s;
	nums[l+1] += d-s;
	nums[r+1] -= d+e;
	nums[r+2] += e;
}
void build(){
	for(int i=1; i<=len; i++)
		nums[i] += nums[i-1];
	
	for(int i=1; i<=len; i++)
		nums[i] += nums[i-1];
}
void result(long long &cur, long long &maxnum){
	cur = nums[0];
	maxnum = nums[0];
	for(int i=1; i<=len; i++){
		cur = cur ^ nums[i];
		maxnum = max(nums[i], maxnum); 
	}
}
int main(){
	
	scanf("%lld", &len);

	long long n;	//n行数据
	scanf("%lld", &n);
	long long l,r,s,e,d;
	for(int i=0; i<n; i++){
		scanf("%lld", &l);
		scanf("%lld", &r);
		scanf("%lld", &s);
		scanf("%lld", &e);
		d = (e-s) / (r-l);	
		set(l,r,s,e,d);
	}
	build();
	long long cur, maxnum;
	result(cur, maxnum);
	cout<<cur<<" "<<maxnum<<endl;
}
```

