[day1](#day1)
[day2](#day2)
[day3](#day3)
[day4](#day4)
[day5](#day5)
[day6](#day6)
#### day1

1. `auto i=min_element()`返回的是迭代器

2. `vector<int> v1(100)`可以变换为`int c[100];` 然后非0复制到`vector`

3. `map`的遍历方法

   ```c++
   for(auto it : map1){			//C++11以后
   	cout << it.first <<" "<< it.second <<endl;
   }
   
   map<string, int>::iterator it;		//C++98
   for (it = m2.begin(); it != m2.end(); it++) {
       string s = it->first;
       printf("%s %d\n", s.data(), it->second);
   }
   ```

4. `map`找到最小值`value`

   ```c++
   bool cmp_value(const pair<int, int> left,const pair<int,int> right){
   	return left.second < right.second;
   }
   
   auto i= max_element(test.begin(),test.end(),cmp_value);
   ```

5. ```c++
   unordered_map<int, int> cnt;
   for(auto x : deck) cnt[x]++;
   for(auto i: cnt) vector.push_back(i.second);
   ```

6. `gcd()`函数，初始化

7. ```c++
   lcm=lcm*a[j]/gcd(lcm,a[j]); // 求最小公倍数
   ```

8. 只需要找到出现次数的最大公约数就可以，和代表数字没有关系

#### day2

1. `vector`的`accumulate()`函数

2. `find`返回的是迭代器，返回不等于`end()`就是找到了，比`cnt.count(i)`要快一点

3. `map`的插入

   ```c++
   m1.insert(make_pair(1,10));
   ```

4. 从头向后遍历，每找到一个特殊的段落，怎么进行处理

5. `vector< pair<int,int> >`这里的格式需要空一格

#### day3
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

7. 利用`max_element()`之后可以`-begin()`求得下标

#### day4

1. vector的位置插入

2. **数字相加的类型总结**

3. vector颠倒顺序

#### day5

1. 不用判断map是否第一次出现，直接++，用数学公式统计次数

2. ```c++
   map<vector<int>,int> mp;
   for(auto it: dominoes){
       if(it[1]<it[0]) swap(it[0],it[1]);  //小的在前
           mp[it]++;
   }
   for(auto it2:mp){
       if(it2.second>1)
           sum+=it2.second*(it2.second-1)/2;
   }
   ```

3. 用到左边累计乘积的概念

#### day6

1. 贪心算法的区间问题，`cmp`排序函数

2. 贪心算法：一对一的分配问题，要考虑好，谁是外面一层循环的问题

3. 
   ```
   |---------|					//贪心的区间问题，特例
	   |-----------------|
	    |--|  |-----|
     