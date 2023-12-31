# 第二章 变量和基本类型

## 2.1 基本内置类型

1. long long是C++11新定义的，unsigned int就是无符号数

2. 但是char被分为了char、signed char和unsigned char

   > 字符型的表现形式只有两种，char实际表现为哪种形式是由编译器决定的
   >
   > 所以算术表达式中尽量不要使用char，可能在一些机器上是有符号的，另一些机器是无符号的

3. 执行浮点数运行优先选用double

```c++
bool b = 42;	//b为真
int i = b;
i = 3.14;
double p = i;
unsigned char c = -1;		//假设char占8比特，c为255
signed char c2 = 256;		//假设char占8比特，c2的值未定义
```

> 当我们赋给**无符号数**一个超出它范围的值时，初始值 = 对数值总数取模后的余数
> 当我们赋给**有符号数**一个超出它范围的值时，它的值是不被定义的



4. 含有无符号类型的表达式，有符号数优先转为无符号数，无符号数永远不会小于0，在循环条件判断时要注意
5. 字符和字符串字面值：编译器在每个字符串的结尾处添加一个空字符('\0')，因此字符串字面值的实际长度要比它的内容多1



## 2.2 变量

1. 对象是指一块能存储数据并具有某种数据类型的内存空间
2. 初始化和赋值是两个完全不同的操作
3. **C++11特性**：用花括号来初始化变量——列表初始化，有丢失数据的风险时，编译器将报错不执行

```c++
double num = 3.14;
int a{num}, b{num};		//报错，有丢失数据的风险
int a(num), b = num;	//正确，但是丢失了一部分数值
```



### 2.2.1 变量声明和定义的关系

1. 声明规定了变量的类型和名字，但定义还会申请存储空间，也可能会为变量赋一个初始值

```c++
extern int i;		//声明i
int j;				//定义j
```

2. 但是不要显式的初始化变量

```c++
extern int j = 3;		//定义j，而不是声明j
```

> 变量只能被定义一次，但是可以被声明多次
>
> 在函数体内部，如果试图初始化一个由extern关键字标记的变量，将引发错误



### 2.2.2 名字的作用域

1. 作用域中一旦声明某个名字，在其嵌套的所有作用域中都能访问该名字，同时允许在内层作用域中重新定义外层作用域已有的名字



## 2.3 复合类型

### 2.3.1 引用

```c++
int ival = 1024;
int &num = ival;		
int &num2;			//报错，引用必须被初始化
```

> 程序把引用和它的初始值绑定在一起，而不是将初始值拷贝给引用
>
> 因为引用本身不是一个对象，它只是为一个已经存在的对象起的另一个名字，所以不能定义引用的引用
>
> 引用只能绑定在对象上，不能与某个字面值绑定在一起



### 2.3.2 指针

- **指针和引用的不同点**
  - 指针本身就是一个对象，允许对指针赋值和拷贝，生命周期内可以先后指向不同的对象
  - 指针无需在定义时赋值
  - 一旦定义了引用，就不能令其再绑定到其他对象

```c++
int *a, *p;
int b = 10;
int &c = b;
a = &b;
int *d = &b;
p = a;

cout<<*a<<endl;

//同时，指针和它所指向的对象要严格匹配
//解引用操作只适用于那些确实指向了某个对象的有效指针
```

```c++
//a = &c;		//错误，不能定义指向引用的指针
```



1. 空指针的几种生成方法

```c++
int *p1 = nullptr;		//等价于int *p1 = 0;	
//C++11新标准

int *p2 = 0;		
int *p3 = NULL;		//头文件cstdlib
```

2. 指针也可以用在if判断条件中

```c++
int *p1 = 0;
int *p2 = &num;

if(p1)		//false
if(p2)		//true
```

3. `void*`指针，可用于存放任意对象的地址，但是我们并不知道地址中是什么类型的操作对象，所以不能直接操作`void*`指针所指的对象
4. 指向指针的指针

```c++
int main() { 
	int a = 10;
	int *p1 = &a;
	int **p2 = &p1;

	cout<<"p1 "<<p1<<endl;
	cout<<"*p1 "<<*p1<<endl;
    
	cout<<"p2 "<<p2<<endl;
	cout<<"*p2 "<<*p2<<endl;
	cout<<"**p2 "<<**p2<<endl;
	return 0;
} 

p1		0x7fff3c91c4f8		//p1内存单元中存放的a的地址
*p1		10					
p2		0x7fff3c91c4f0		//p2内存单元中存放的p1单元的地址
*p2		0x7fff3c91c4f8		//*p2解一层引用，为p1单元内的值
**p2	10					//解两层引用，找到a的值
```

5. 指向指针的引用

> 不能定义指向引用的指针，但是可以定义指针的引用

```c++
int i = 10;
int *p;
int *&r = p;		//对指针p的引用

r = &i;			//等价于令p指向i
*r = 0;			//将i的值改为0
```

> *&r，从右往左阅读，离变量名最近的符号对变量的类型有最直接的影响



## 2.4 const限定符

- const对象必须初始化，创建后其值就不能再被改变了
- 当多个文件中出现同名的const变量时，等同于在不同文件中分别定义了独立的变量
- extern const——只在一个文件中定义，可以在其他文件中使用同一个变量

```c++
//file1.cpp 定义并初始化了一个常量，该常量能被其他文件访问
extern const int buffsize = 512;

//file1.h 头文件，与file1.cpp中的变量是同一个
extern const int buffsize;
```



### 2.4.1 const的引用

```c++
const int ci = 1024;
const int &p = ci;		//正确，引用及其对应的对象都是常量
p = 42;					//错误，p是对常量的引用
int &p2 = ci;			//错误，试图让一个非常量引用指向一个常量对象
```

> 前文提到引用的类型必须与其所引用对象类型一致，但这里存在两个例外

```c++
int i = 42;
const int &r1 = i;				//允许，但不能通r1修改i的值，例外
const int &r2 = 42;				//允许
const int &r3 = r1*2;			//允许
int &r4 = r1*2;					//错误
```

```c++
double num = 3.14;
const int &r = num;				//允许，例外

//编译器做法
//const int temp = num;
//const int &r = temp;
```



### 2.4.2 指针和const

> 要想存放常量对象的地址，只能使用指向常量的指针

```c++
const double pi = 3.14;
double *ptr = &pi;				//错误
const double *cptr = &pi;		//正确
*cptr = 42;					//错误，不能修改
```

> 前面提到指针的类型必须和所指对象的类型一致，但是也存在两种例外

```c++
const double *cptr;
double num = 3.12;
cptr = &num;			//允许，例外，但不能通过cptr改变num的值
```



1. 指针和const

> const在*的左侧，指const用来修饰指针所指向的变量，不能改变指针的解引用
>
> const在*的右侧，指const本身就是修饰指针本身，即指针本身是常量，不能修改指针指向地址

```c++
const char *a;		//指向常量的指针，不能改变解引用
char const *a;		//同上
char *const a;		//常指针、const指针，不能改变地址
const chat *const a;//指向const对象的const指针
```


