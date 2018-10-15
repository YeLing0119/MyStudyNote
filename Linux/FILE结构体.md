## FILE  结构体

​	大概原型

​	typedef struct iofile{

​		int cnt ; 		//还剩下多少字节没有读

​		char *base ;   //文件的起始地址

​		char *ptr ;	//文件读取的当前位置

​		int fd ;

​	}

