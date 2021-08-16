# Mymuduo
一个基于reactor模式的多线程并发服务器模型的简单实现，学习来源于陈硕的muduo，摆脱了boost库的依赖。 
### 项目表述
该项目主要实现了在一个Reactor的模式下的Channel-EventLoop-Epoller的多线程并发服务器  
采用了Epolle的LT模式，作为IO复用的模型，在应用层的层面上加入了输入和输出的缓冲区， 
使用了ThreadPool来进行任务的处理，和注册的定时器Timer类，TimerQueue类  
### 使用过程
在文件夹下打开
```
cd Mymuduo
make clean
make
./Mymuduo
```
启动另个一终端输入   
```
nc 127.0.0.1 10037
```
就可以开始通信了   
### 主要结构简介   
1.声明与接口类:IMuduoUser,IChannelCallback,IAcceptorCallback,IRun,Define.h,Declear.h  
2.时间相关类:Timestamp,Timer,TimerQueue  
3.条件变量锁相关类:MutexLock,MutexLockGuard,Condition,BlockQueue  
4.任务线程类:Task,Thread,ThreadPool  
5.Reactor三剑客类:EventLoop,Channel,Epoll  
6.接受与连接类:Acceptor,TcpConnection  
7.终端服务器类:TcpServer,EchoServer  
8.Makefile及Readme  
### 声明与接口类
### 时间相关类
时间相关主要有三个类别Timestamp时间戳类，Timer定时器类，TimerQueue定时器队列   
**1.Timestamp时间戳类** 采取了六十四位的int数据来储存时间，实现了获取当前时间，时间比较判断时间是否有效的方法。  
**2.Timer是定时器类**

### 条件变量锁相关类
条件变量和锁主要有四个类别MutexLock互斥量,MutexLockGuard互斥量实例RAII,Condition条件变量类,BlockQueue无界的缓冲区  
**1.MutexLock互斥量：**  
由于我们是线程来执行任务的，我们的互斥量内部采用的是pthread_mutex类别的  
构造函数调用pthread_mutex_init,析构函数调用pthread_mutex__destory  
在其内部lock与unlock 分别调用pthread_lock/unlock  
**2.MutexLockGuard互斥量实例：**  
采取的是RAII封装技法，在MutexLockGuard类中包含了一个Mutex对象  
在构造时进行创建加锁，在析构时自动解锁推出  
**3.Condition条件变量类：**   
条件变量类内部也维护了一个pthread_cond_it也自动的调用内部的互斥量进行wakeup和signal操作  
**4.BlockQueue无界的缓冲区：**
无界缓存区类则是完全的模拟了生产者和消费者的模型，put和take方法打配内部的条件变量，  
当缓冲区为空的时候不能进行take操作wait阻塞线程，当put放置任务时signal唤醒线程   
### 任务线程类
任务和线程类主要有以下的三种类别Task任务类,Thread线程类,ThreadPool线程池类  
**1.Task任务类：**  
重载了两个构造函数，模拟了读写过程锁执行的两种任务  
**2.Thread线程类：**  
完成了pthread_create的创建线程并执行任务的功能，还提供了获取tid的方法  
**3.ThreadPool线程池类：**  
内部有一个队列任务池和一个vector线程池，threadpool进行start方法后线程就会到任务池区领取任务并执行  
### Reactor三剑客类
Reactor模式在本项目的实现主要由三个类来实现，
### 接受与连接类
### 终端服务器类
