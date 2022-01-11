## 多线程基础

### 线程管理基础

进程间通信

线程间通信 ： 开销小，但是共享空间需要保护

```c++
#include <thread>
std::thread t(fun);//创建的时候就表示启动了线程
if(t.joinable()) // 只能join一次，没有join的话 下一步join
    {
      t.join();
  // 等这个线程结束 或者t.detach() 不等
  //使用detach()会让线程在后台运行，主线程不能与之产生直接交互。也就是说，不会等待这个线程结束；如果线程分离，那么就不可能有std::thread对象能引用它
    }

```

需确保线程在函数之前结束 所以可以使用一些方法，使用“资源获取即初始化方式”(RAII，Resource Acquisition Is Initialization)，并且提供一个类，在析构函数中使用**join()**

```c++
class thread_guard
{
  std::thread& t;
public:
  explicit thread_guard(std::thread& t_):
    t(t_)
  {}
  ~thread_guard()
  {
    if(t.joinable()) // 1
    {
      t.join();      // 2
    }
  }
  thread_guard(thread_guard const&)=delete;   // 3
  thread_guard& operator=(thread_guard const&)=delete;
};

struct func; // 定义在清单2.1中

void f()
{
  int some_local_state=0;
  func my_func(some_local_state);
  std::thread t(my_func);
  thread_guard g(t);
  do_something_in_current_thread();
}    // 4
```

#### 后台运行线程

当`std::thread`对象使用t.joinable()返回的是true(说明没有join过，可以)，就可以使用t.detach()。不过C++运行库保证，当线程退出时，相关资源的能够正确回收，后台线程的归属和控制C++运行库都会处理。

通常称分离线程为*守护线程*(daemon threads),UNIX中守护线程是指，没有任何显式的用户接口，并在后台运行的线程。这种线程的特点就是长时间运行；线程的生命周期可能会从某一个应用起始到结束

```c++
std::thread t(do_background_work);
t.detach();
assert(!t.joinable());//assert语法
```



### 向线程函数传递参数

默认参数要拷贝到线程独立内存中，即使参数是引用的形式，也可以在新线程中进行访问

```c++
void f(int i,std::string const& s);
void oops(int some_param)
{
  char buffer[1024]; // 1
  sprintf(buffer, "%i",some_param);
  std::thread t(f,3,buffer); // //可能会崩溃
  std::thread t(f,3,std::string(buffer));  // 使用std::string，避免悬垂指针。在传递到std::thread构造函数之前就将字面值转化为std::string对象：
  t.detach();
} 
```

thread构造函数无视函数期待的参数类型，并盲目的拷贝已提供的变量，因此在3将会接收到没有修改的data变量

```c++
void update_data_for_widget(widget_id w,widget_data& data); // 1
void oops_again(widget_id w)
{
  widget_data data;
  std::thread t(update_data_for_widget,w,data); // 2
  display_status();
  t.join();
  process_widget_data(data); // 3
}
```

也可以传递类成员函数。也可以为成员函数提供参数。

```c++
class X
{
public:
  void do_lengthy_work(int);
};
X my_x;
int num(0);
std::thread t(&X::do_lengthy_work, &my_x, num);//num就是do_lengthy_work的参数。my_x的地址作为指针对象提供给函数。
//my_x.do_lengthy_work()作为线程函数；
```

提供的参数可以移动，但不能拷贝

在`std::thread`的构造函数中指定`std::move(p)`,big_object对象的所有权就被首先转移到新创建线程的的内部存储中，之后传递给process_big_object函数。

```c++
void process_big_object(std::unique_ptr<big_object>);
std::unique_ptr<big_object> p(new big_object);
p->prepare_data(42);
std::thread t(process_big_object,std::move(p));
```



### 转移线程所有权

```c++
void some_function();
void some_other_function();
std::thread t1(some_function);            // 1
std::thread t2=std::move(t1);            // t1已经和执行线程已经没有关联了，执行some_function的函数现在与t2关联。
t1=std::thread(some_other_function);    // 一个临时std::thread对象相关的线程启动了③。为什么不显式调用std::move()转移所有权呢？因为，所有者是一个临时对象——移动操作将会隐式的调用。
std::thread t3;                            // 4
t3=std::move(t2);                        // 移动操作完成后，t1与执行some_other_function的线程相关联，t2与任何线程都无关联，t3与执行some_function的线程相关联。
t1=std::move(t3);                        // 6 赋值操作将使程序崩溃
```

### 运行时决定线程数量

std::thread::hardware_concurrency()

```c++
//实现 并行版的std::accumulate
不太理解，可能就是为了确定当前使用多少个线程合适
```

### 识别线程

```c++
std::thread::id
  //第一种，可以通过调用std::thread对象的成员函数get_id()来直接获取
  //第二种，当前线程中调用std::this_thread::get_id()
std::thread::id master_thread;
void some_core_part_of_algorithm()
{
  if(std::this_thread::get_id() == master_thread)
  {
    do_master_thread_work();
  }
  do_common_work();
}
```





## 线程间共享数据
