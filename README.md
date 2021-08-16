# Mymuduo
一个基于reactor模式的多线程并发服务器模型的简单实现，学习来源于陈硕的muduo，摆脱了boost库的依赖。 
### 项目表述
该项目主要实现了在一个Reactor的模式下的Channel-EventLoop-Epoller的多线程并发服务器  
采用了Epolle的LT模式，作为IO复用的模型，在应用层的层面上加入了输入和输出的缓冲区  
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
### 文件的主要结构   
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
Timestamp时间戳类 采取了六十四位的int数据来储存时间，实现了获取当前时间，时间比较判断时间是否有效的方法。  
Timer是定时器类

### 条件变量锁相关类
条件变量和锁主要有四个类别MutexLock互斥量,MutexLockGuard互斥量实例RAII,Condition条件变量类,BlockQueue无界的缓冲区
### 任务线程类
### Reactor三剑客类
### 接受与连接类
### 终端服务器类
