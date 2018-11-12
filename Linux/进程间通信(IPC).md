## 程间通信（IPC）

#### 为什么要进程通信？

​	1、数据传输

​		一个进程需要将它的数据发送到另一个进程

​	2、资源共享

​		多个进程之间共享同样的资源

​	3、通知事件

​		一个进程要向另一个程序发送通知消息，通知它们发生了某种事件，比如子进程终止时要通知父进程

​	4、进程控制

​		有些进程希望完全控制另一个进程的执行（比如 ： DEGUG），

​		此时控制进程希望能够拦截另一个进程的所有陷入和异常，并能够知道他们状态的改变。

#### 进程间通讯的发展

​	1、管道

​	2、System V	IPC

​		消息队列（数据传输）

​		共享内存（资源共享）

​		信号量（事件通知）

​	3、POSIX   IPC

​		消息队列

​		共享内存

​		互斥量

​		条件变量

​		信号量

​		读写锁

#### 管道

​	半双工的通讯方式

​	```int  pipe (int fds[2])；``` 

​	

![](./管道.jpg)

一个例子

​	[利用管道函数pipe实现进程间通讯](https://github.com/YeLing0119/MyCode/tree/master/Linux/pipe)

#### 消息队列

​	1 . 获取/创建消息队列

​		#include \<sys/msg.h\>	

​		#include \<sys/ipc.h>

​		int msgget(key_t key ,  	//相当于文件名

​				int flags)	;	//	创建  IPC_CREAT | 0644(权限)	//	打开   0	

​	返回值：消息队列的id ，相当于文件描述符

​	查看IPC对象 

​		ipcs  -q  //查看消息队列   查看IPC对象

​		ipcrm  -Q  key   //删除指定key的消息队列  删除IPC对象

​		系统中最多可以创建多少个消息队列?

​		cat /proc/sys/kernel/msgmni	//最多创建个数

​		cat /proc/sys/kernel/msgmax	//一条消息最多装多少字节

​		cat /proc/sys/kernel/msgmnb	//一个消息队列中一个消息的总字节数是多少

​	2 . 发送消息

​		int msgsnd(int msgid , 		//msgget的返回值

​			const void* msgp , 	//消息的位置		必须是类似于msgbuf的结构体

​			size_t len , 		//消息的长度		不包括type的大小

​			int flag) ;			//消息的标志位

​	返回值	： -1	//失败		0	//成功

​		struct msgbuff{

​			long type ; 		//消息类型	必须 >= 1	必须有

​			//随便			随意类型，写上自己的消息

​		}	

​	3 . 接收消息

​		ssize_t msgrcv(int msgid , 		//msgget的返回值

​			void *msgp , 	//取出消息存放位置

​			size_t msgsz , 	//装消息位置的大小，不包括类型

​			long msgtype , 	//取那个类型的消息

​			int msgflag);		//选项，一般填0 ， 表示没有消息就在等，有了就取出来

​	返回值：-1	失败   	成功实际拷贝的字节数

​	

#### 共享内存

​	1 . 创建或者打开共享内存

​		shmget(key_t key , 	//相当于文件名

​			size_t size,	//共享内存的大小

​			inr flag);		//创建IPC_CREAT|0664	打开0

​	返回值：-1  	 失败    0		成功

​	2 . 让共享内存与本进程建立关系

​		void  *shmat(key_t key , 

​			const char* shmaddr ,   //让操作系统挂载到地址空间的位置  若为NULL，这让操作系统自己选择 

​			int flag);				// 0

​	返回值：实际挂载到虚拟地址空间的起始位置

​	3 . 卸载掉共享内存

​		int   shmdt(void * shmaddr)	;	

​	4 . 删除共享内存

​		int shmctl(key_t key , 

​					int cmd , 	//	IPC_RMID	删除共享内存

​					NULL)		//本来这里是个结构体，但是删除用不上，直接填NULL

