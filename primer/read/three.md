# 第三章 字符串、向量和数组

## 3.1 string

### 3.1.1 定义和初始化

```c++
string s1(s);	//直接初始化
string s2 = s1;		//拷贝初始化
string s3(n, 'c');
string s4("hello");
```

### 3.1.2 对象的操作

- 读取操作时，string对象会自动忽略开头的空白，直到遇见下一处空白为止

```c++
while(cin>>s)	//可以执行重复输入

getline(cin, s);	//读入一行
//直到遇到换行符为止(换行符也被读进来了)，然后把所读的内容存入到string对象中去(不存入换行符)
```



- s.size()函数返回的是一个无符号的整型数

```c++
s.size() < n;	//若n是负数，则比较会自动转换为一个较大的无符号数，所以此表达式一定为trye
```



- 字面值和string对象相加

```c++
string s1 = s + "!";		//正确
string s2 = s1 + s;		//正确
string s3 = "hello" + "world";		//错误，不能把字面值直接相加赋给string对象
string s4 = s1 + "hello" + "world";		
//正确，等价于(s1+"hello")+"world"

string s5 = "hello" + "world" + s1;		//错误，等价于("hello"+"world"+s1)
```

**字符串字面值与string是不同的类型**



### 3.1.3 处理字符

- 处理每个字符

```c++
for(auto s: s1)
```

- 改变字符串中的字符

```c++
for(auto &s : s1)
//要想改变string对象中字符的值，必须把循环变量定义成引用类型
```

- 可以用下标访问

**C++语言规定只有当左侧运算符对象为真时才会检查右侧运算对象的情况**



## 3.2 vector

### 3.2.1 定义和初始化

- 特殊，列表初始化

```c++
vector<string> s={"a", "b"};
//列表初始化，用圆括号即为错误

vector<int> v1(10);		//10个元素
vector<int> v2{10};		//1个元素

vector<string> s1{"hello"};		//1个元素
vector<string> s2("hello");		//错误，直接初始化是对象
vector<string> s3(10);			//10个默认初始化的元素
vector<string> s4{10, "hi"};	//10个hi，和int的区别
```

- **例外情况**

> 拷贝初始化时，只能提供一个初始值