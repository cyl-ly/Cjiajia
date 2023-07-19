## vector

1.表示大小可变的数组

2.采用连续存储空间来存储元素，可以用下标对数组进行访问

3.使用动态分配来存储元素，当插入元素导致空间不够使，会开辟新的数组，大小一般变为二倍

4.但是头插法效率较低，经常使用删除操作，应该使用list容器

## vector定义

| 构造函数                                            | 说明                     |
| --------------------------------------------------- | ------------------------ |
| vector()                                            | 无参数构造               |
| vector(type_n, const value_type &val=value_type() ) | 构造并初始化n个val       |
| vector(const vector &x)                             | 拷贝构造                 |
| vector(first, last)                                 | 使用迭代器进行初始化构造 |

## vector的使用

1.正向迭代器

> `begin()`返回指向第一个元素的迭代器
>
> `end()`返回指向最后一个元素的下一个迭代器

2.反向迭代器

> `rbegin()`返回指向最后一个元素的迭代器
>
> `rend()`返回指向第一个元素的前一个迭代器

> 迭代器：提供一种方法来访问聚合对象中的各个元素，而不用暴露这个对象的内部表示

## iterator

`iterator`可以看作STL容器的指针，可以更为方便的访问容器里的元素

```c++
vector<int>::iterator = it;	
//声明了一个名为it的变量，他的数据类型是由vector<int>定义的iterator类型
```

```c++
#include<iostream>
#include<vector>
using namespace std;

int main(){
    vector<int> v1(10,1);			//10个1

    int b[7]={2,2,3,3,4,4,4};
    vector<int> v2(b,b+7);			//2,2,3,3,4,4,4 
    //从下标0开始的位置，插入(7-0=7)个元素

    vector<int> v3;
    v3.assign(v2.begin(),v2.begin()+3);		//2,2,3
    v3.back();				//返回最后一个元素，不是迭代器
    v3.front();				//第一个元素
    v3.clear();				//清空
    v3.empty();				//空则返回true
    v3.pop_back();			//删除向量的最后一个元素
    v3.push_back(10);		//10插入为最后一个元素
    v3.size();				//返回元素个数
    v3.capacity();			//返回容量大小
    v3.resize(10);			//个数调整为10个，值随机
    v3.resize(10,2);		//个数调整为10个，值为2
    v3.reserve(100);		//把容量扩充到100
    v3.swap(v2);			//整体交换元素
    v3==v2					//判断语句，!=,>=,<=,>,<
   
    v3.erase(v3.begin()+2, v3.begin()+3);	
    //在v3下标为2的位置,删除(3-1=2)个元素
    v3.insert(v3.begin()+2,10); 
    //在v3下标为2的位置插入数值10,其他后移，如a为1,2,3,4，插入元素后为1,2,10,3,4
    v3.insert(v3.begin()+1,3,5); 
    //在v3下标为1的位置插入3个数,其值都为5
    v3.insert(v3.begin()+1,b+3,b+6); 
    //b为数组,在v3下标为1的位置插入,b下标为3开始的三(6-3)个元素
    
    vector<int>::iterator it;

    for(it=v2.begin();it!=v2.end();it++){
        cout<<*it<<" ";
    }
    return 0;
}
```



```c++
#include<iostream>
#include<vector>
using namespace std;

int main(){
    vector<int> v1;			//无参数构造
    vector<int> v2(3,10);	//用3个10来构造
    vector<int> v3(v2.begin(), v2.end());	//迭代器区间构造
    vector<int> v4(v3);		//拷贝构造
    
    int arr[]={1,2,3,4,5,6,7,8,9};
    vector<int> v5(arr, arr+sizeof(arr)/sizeof(arr[0]));
    //1,2,3,4,5,6,7,8,9
    //从下标0开始，插入(9-0=9)个元素，36/4=9
    
    vector<int>::iterator it=v5.begin();
    for(;it!=v5.end();it++)
       cout<<*it<<endl;
    
    vector<int>::iterator it;
    for(it=v5.begin();it!=v5.end();it++)
       cout<<*it<<endl;
    
    for(auto e : v5)
		cout << e << " ";		//此方法同样可以遍历
}
```

- reserve只负责开辟空间，如果确定知道需要用多少空间，reserve可以缓解vector增容的代价缺陷问题

- resize在开空间的同时还会进行初始化，如果开辟的比原来小则会删除后面的数据，影响size

- reserve只会增大capacity。如果再减少，capacity不会改变，这样是防止一会儿你又要扩容

- ```c++
  v5.reserve(100);
  cout<<v5.capacity()<<endl;		//100
  v5.reserve(10);
  cout<<v5.capacity()<<endl;		//100
  ```

## 增删改查

补充以上简短语法

| 增删改查    | 说明                                                 |
| ----------- | ---------------------------------------------------- |
| `find`      | 是算法模块实现，不是vector的成员接口，**返回迭代器** |
| `opreate[]` | 像数组一样访问                                       |

- ```c++
  vector<int>::iterator pos=find(v5.begin(),v5.end(),5);
  v5.erase(pos);
  ```

### `opreate[]`与`at()`函数的区别

1. 调用`at()`方法，如果下标越界，会使程序抛出一个`std::out_of_range`的异常。

   而调用`operator[]`方法，如果下标越界，程序会崩溃

2. `array、deque、vector`不能通过`operator[]()`向容器中添加元素

   `map、unordered_map`类可以通过`operator[]`向容器中添加元素

3. 以上容器均不能通过`at()`函数向容器重添加元素

4. `at()`函数在被调用时，会检查下标的有效性（与容器的`size()`比较而不是`capacity()`（例如`vector`）），若下标有效则返回对应位置的元素，否则抛出`std::out_of_range`异常。

   `operator[]()`函数在被调用时，不检查下标的有效性

5. 例外：对于`array`而言，无论使用`operator[]`还是`at()`都会做下标有效性检查

## vector源码解析

