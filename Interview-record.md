### [C++](https://cplusplus.com/reference/)
- [x] 野指针 & 动态指针
    - 参考：https://zhuanlan.zhihu.com/p/150926332
    - 造成野指针的原因
        - 指针变量未初始化
        - 指针delete或者free之后没有值为空
        - 指针超出了变量的作用范围
        ```
        //原因一,指针变量没有初始化,包括类成员变量的初始化;
        int* p;//错误;
        int* p=NULL;
        class Sample
        {
            int *p;
        public:
            Sample(){} //没有在构造函数初始化，这个错误一个不小心就出现了;
            int add(int x,int y) {return x+y;}
        };

        //原因二,指针delete或者free之后没有值为空
        Sample* sample=new Sample();
        Caller.setSample(sample);//这个指针变量赋值给一个Caller类对象的某个成员变量;
        sample->add(4,5);
        delete sample;//delete之后所指向的内容为空，但是自身地址还在,如果没有
        if(sample!=NULL)
        {
            //执行代码;
        }

        //原因三，超过变量的作用范围
        int* test()
        {
            int val=3;//局部变量，在函数执行完成之后，val变量会被析构，这个时候返回的指针就是野指针
            return &val;
        }

        //原因四,内存被其他数据改动;
        Sample* sample=new Sample();
        //正常情况下基本没有这种操作模式，但是也有个别设计会用到（如通过偏移地址给私有类变量赋值）
        char* temp_str="abcdefc";
        memcpy(sample,temp_str,sizeof(temp_str);
        ```
    - 野指针的排查工具： valgrint的memcheck工具来检查
        ```
        g++ mem.cpp -o mem -g
        valgrint --tool=memcheck ./mem
        ```
    - 智能指针
        - 主要用于管理在堆上分配的内存，它将普通的指针封装为一个栈对象。当栈对象的生存周期结束后，会在析构函数中释放掉申请的内存，从而防止内存泄漏。
        - C++ 11中最常用的智能指针类型为shared_ptr (引用计数的方法，记录当前内存资源被多少个智能指针引用)
        - 该引用计数的内存在堆上分配。当新增一个时引用计数加1，当过期时引用计数减一。只有引用计数为0时，智能指针才会自动释放引用的内存资源
- [x] 哪种函数调用不增加栈
  - 内联函数(inline修饰)
      - 编译时，编译器将调用内联函数的地方直接替换成内联函数里的代码逻辑
      - 限制：inline只适合函数体内代码简单的函数使用，不能包含复杂的结构控制语句例如 while、switch，并且不能内联函数本身不能是直接递归函数。
      - 建议：inline 函数仅仅是一个对编译器的建议
      - 内联函数的定义必须出现在内联函数第一次调用之前；且建议 inline 函数的定义放在头文件中
      ```
      #include <iostream>
      using namespace std;

      inline int Max(int x, int y){
        return (x > y)? x : y;
      }

      // 程序的主函数
      int main( ){
        cout << "Max (20,10): " << Max(20,10) << endl;
        cout << "Max (0,200): " << Max(0,200) << endl;
        cout << "Max (100,1010): " << Max(100,1010) << endl;
        return 0;
      }
      ```
- [ ] GCC 编译命令；包含非当前目录的.h文件 + 包含第三方so库 + makefile
- [ ] C++语言的编译过程
- [ ] GDB调试 + core问题的排查
- [ ] 多线程下的线程锁
- [ ] 如何写一段多线程
- [ ] 内存溢出问题
- [ ] i++ = 1 和 ++i = 1 哪个会出问题

### Golang

### 网络编程
- [ ] 网络通信模型：TCP三次握手 + 四次挥手
- [ ] 如何实现一个长连接
- [ ] 通信协议；http
- [ ] 其他的rpc协议：gRPC

### Linux编程
- [ ] 可执行文件格式
- [ ] linux中如何执行一个可执行文件


### 多线程
- [ ] 线程锁


### 要求
  - 计算机基础：网络、操作系统、数据库
    - 有精力可以温习一遍CSAPP（《深入理解计算机系统》）
  - 至少熟练掌握一门后端语言的开发框架，如
    - Java：Spring
    - Python：Django、Tornado、Flask
    - Golang：Gin、Revel
  - 常用中间件:  RPC框架(gRPC、dubbo)、消息队列等
  - 容器技术：Docker、K8S
  - 常见设计模式
    - 有精力可以温习一遍GoF（《设计模式》）