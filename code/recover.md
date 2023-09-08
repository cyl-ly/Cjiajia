[day1](#day1)
[day2](#day2)

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