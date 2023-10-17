## 单调队列和优先级队列

> “如果一个人比你年轻还比你强，那你就要被踢出去了……”——单调队列
>
> “来来来，神犇巨佬、金牌Au爷、AKer站在最上面，蒟蒻都靠下站！！！”——优先队列

#### 单调队列

- **单调队列** 就是能够完美支持下面三种操作的一种容器：

    1）【询问】通过O1的时间，获取容器中元素的最大值。

    2）【删除】通过O1的时间，删除元素；
    
    3）【插入】通过O1的时间，插入元素；

- 单调队列是一个限制只能 **队尾插入**，但是可以 **两端删除** 的 **双端队列** 。**单调队列** 存储的元素值，是从 **队首** 到 **队尾** 呈单调性的（要么单调递增，要么单调递减）。对于求解最大值的问题，则需要维护一个 **单调递减** 的队列。

```c++
class mydeque{
    //利用双端队列实现单调队列
    //对外接口pop push getmax
    //每次插入元素的时候，要保证“当前始终”是最大的，其余的从队尾弹走
    //当要pop最大值的时候，从前面弹走
    public:
        deque<int> md;
        void pop(int key){
            if(!md.empty() && key == md.front())
                md.pop_front();     //要弹走的元素是最大值，才会在前面弹出
        }
        void push(int key){
            while(!md.empty() && md.back() < key)
                md.pop_back();      //插入时，比自己小的后面弹走
            md.push_back(key);
        }
        int getmax(){
            return md.front();
        }
    };
```

#### 优先级队列

- 优先级队列每次出队的元素是队列中优先级最高的那个元素，而不是队首的元素。这个优先级可以通过元素的大小等进行定义。比如定义元素越大优先级越高，那么每次出队，都是将当前队列中最大的那个元素出队。
- 现在看**优先级队列**是不是就是**“堆”**了，如果最大的元素优先级最高，那么每次出队的就是当前队列中最大的元素，那么队列实际就相当于一个大顶堆，每次将堆根节点元素弹出，重新维护大顶堆，就可以实现一个优先级队列。

```c++
//声明大根堆的形式
//priority_queue的成员函数：empty、size、pop、top、push

priority_queue< type, container, function>;
//这三个参数，后面两个可以省略，第一个不可以。其中
//type:数据类型
//container:实现优先队列的底层容器，必须是可随机访问的容器，例如vector、deque，而不能使用list
//function:元素之间的比较方式

priority_queue<int> big_heap;
priority_queue<int,vector<int>,less<int> > big_heap;
priority_queue<int,vector<int>,greater<int> > small_heap;
```

```c++
struct cmp{
// 大根堆
	bool operator ()(const pair<int, int>& a, const pair<int, int>& b){
		return a.second < b.second; 	//可以自己改变去根据什么排序
	}
};

priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> heap;

priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int,int>>> heap;	
//是根据第一个数进行排序的
```


