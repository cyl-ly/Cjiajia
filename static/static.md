当与不同的类型一起使用时，会有不同的含义

（1）**函数中的静态变量**

当变量声明为static时，空间**将在程序的生命周期内分配**。即使多次调用该函数，静态变量的空间也**只分配一次**，前一次调用中的变量值通过下一次函数调用传递。这对于在C / C ++或需要存储先前函数状态的任何其他应用程序非常有用。

```c++
#include <iostream> 
#include <string> 
using namespace std; 

void demo() { 
	static int count = 0; 
	cout << count << " "; 
	count++; 
} 

int main() { 
	for (int i=0; i<5; i++)	 
		demo(); 
	return 0; 
} 
```

> 输出 `0 1 2 3 4`
>
> 值通过函数调用来传递。每次调用函数时，都不会对变量计数进行初始化

（2）**类中的静态变量**

由于声明为static的变量只被初始化一次，因为它们在单独的静态存储中分配了空间，因此类中的静态变量**由对象共享**。

类的静态数据成员要在类的外部进行初始化，因为静态数据成员不属于某一个对象所有，在内部初始化的话会造成多次初始化现象

```c++
#include<iostream> 
using namespace std; 
class Apple { 
public: 
	static int i; 
	Apple() { 

	}; 
}; 
int Apple::i=1;

int main() { 
	Apple obj1; 
	Apple obj2; 
    cout << obj1.i<<" "<<obj2.i<<endl; 

    obj1.i++;
	cout << obj1.i<<" "<<obj2.i; 
} 
```

> 输出` 1 1 `
>
> ​		` 2 2`

[一个初始化的问题](https://blog.csdn.net/qq_20789179/article/details/103909426)

（3）类对象为静态

```c++
#include<iostream> 
using namespace std; 

class Apple { 
	int i; 
	public: 
		Apple() { 
			i = 0; 
			cout << "Inside Constructor\n"; 
		} 
		~Apple() { 
			cout << "Inside Destructor\n"; 
		} 
}; 

int main() { 
	int x = 0; 
	if (x==0) { 
		Apple obj; 
	} 
	cout << "End of main\n"; 
} 
```

对象在if块内声明为非静态。因此，变量的范围仅在if块内。

> 输出
>
> `Inside Constructor`
> `Inside Destructor`
> `End of main`

```c++
#include<iostream> 
using namespace std; 

class Apple { 
	int i; 
	public: 
		Apple() 
		{ 
			i = 0; 
			cout << "Inside Constructor\n"; 
		} 
		~Apple() 
		{ 
			cout << "Inside Destructor\n"; 
		} 
}; 

int main() { 
	int x = 0; 
	if (x==0) { 
		static Apple obj; 
	} 
	cout << "End of main\n"; 
} 
```

> 输出
>
> `Inside Constructor`
> `End of main`
> `Inside Destructor`

在`main`结束后调用析构函数。这是因为静态对象的范围是贯穿程序的生命周期。

（4）类中的静态函数

就像类中的静态数据成员或静态变量一样，静态成员函数也不依赖于类的对象。我们被允许使用对象和'.'来调用静态成员函数。但建议使用类名和范围解析运算符调用静态成员。

允许静态成员函数仅访问静态数据成员或其他静态成员函数，它们无法访问类的非静态数据成员或成员函数。

```c++
#include<iostream> 
using namespace std; 

class Apple { 
    public: 
        static void printMsg() {
            cout<<"Welcome to Apple!"; 
        }
}; 

int main() { 
    Apple::printMsg(); 
} 
```

> 输出
>
> `Welcome to Apple!`

（5）限定访问范围

```c++
// source1.cpp
extern void sayHello();
const char* msg = "Hello World!\n";
int main(){
    sayHello();
    return 0;
}

// source2.cpp
#include <cstdio>
extern char* msg;
void sayHello(){
    printf("%s", msg);
}
```

`g++`对于上面两个代码文件是可以正常编译并且打印`Hello World!`，但如果给`source1.cpp`中的`msg`加上`static`，则会导致`undefined reference to 'msg'`的编译错误：