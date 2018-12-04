## IO相关操作
> 对于IO操作而言，有五个基本的操作：open 、read 、write 、close、lseek  我们来逐个解释。

> 在此之前我们先解释一下什么是文件描述符

#### 1. 文件描述符
> 操作系统通过一个整数开代表打开的文件，我们将这个整数称为文件描述符。

> 文件描述符的范围	[ 0 ~  1024 ]  不同的系统可能上限不同  
具体查看方法​	``` ulimit -n ```
- ​0：标准输入  stdin ​
- 1：标准输出  stdout 
- 2：标准错误  stderr
- 系统选择文件描述符的方法，从小到大找第一个未被使用 的文件描述符，一般从3开始  因为0 、1 、 2 已经被系统占用。

#### 2. open
> ```int open(const char *path , int flag);```
- 功能：打开文件
- 参数：
	- path：要打开的文件
	- flags：打开方式
		- O_RONLY：只读方式打开
		- O_WONLY：只写方式打开
		- O_RDWR ：读写方式打开
		- O_CREAT：存在则打开不存在则创建
		- O_EXCL：与O_CREAT同用，存在则打开不存在则报错
		- O_TRUNC：截断文件为大小0
		- O_APPEND：追加
	- 返回值 ： 
		- 失败：-1
		- 成功：文件描述符
> ```int open (const char *path  , int flags , mode_t mode )```
- 功能：创建文件
- 参数：
	- *path：要创建的文件名
	- flags：O_CREAT | O_EXCL 若不存在，则创建
	- mode：给文件的权限(受umask影响)
		- umask(), 在程序中设置当前进程的默认掩码

#### 3. read
> ```int read(int fd , char *buf, size_t len);```	
- 功能 ： 从 fd 文件中读取数据到buf所指向的空间，该空间大小为 len 
- 参数 ：
	- fd ：open 函数返回的文件描述符
	- *buf：要读入数据的空间
	- len：读取的长度
- 返回值 ：
	- 失败：-1
	- 成功：实际读取的字节数
#### 4. write 
> ```int write (int fd , const char *buf  , size_t len );```
- 功能：往文件写入内容
- 参数：
	- fd：文件描述符
	- buff：缓冲区
	- len:要写入的数据长度
- 返回值：
	- 失败:-1
	- 成功:实际写入的字节数
#### 5. close 
> ```int close (int fd ) ; ```
- 关闭文件

#### 6. lseek
> ```int lseek(int fd , off_t offset , int whence)``` 
- 功能：定位到指定位置
- 参数：
	- ​offset ： 偏移量​(可正可负)
	- whence ：
		- SEEK_SET	文件开始
		- SEEK_CUR	文件当前指向
		- SEEK_END	文件结尾
- ​返回值：相对于文件开头的偏移多少字节

#### 7. dup/dup2
- dup(int oldfd)
	- 功能：复制一个文件描述符，返回新的描述符

- dup2(int oldfd，int newfd)
	- 功能：文件重定向，两个文件描述符必须有效，把newfd指向的文件重定向到oldfd所指向的文件

#### 结合以上函数 写一个简单的拷贝函数

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <string.h>
#include <errno.h>

//usage : copy src dst
int main(int argc , char* argv[]){
	if(argc != 3 ){
		fprintf(stderr,"usage : %s src dst \n", argv[0]);
		exit(1);
	}
	//打开源文件
	int fd_src = open (argv[1], O_RDONLY);
	if(fd_src == -1 ) perror("open"),exit(0);
	
	// 打开目标文件 ，若不存在则创建
	int fd_dst = open (argv[2], O_CREAT|O_RDWR|O_EXCL, 0644);
	if(fd_dst == -1 && errno == EEXIST ){
		printf("目标存在，是否覆盖(Y/N)\n");
		char choose ;
		scanf(" %c", &choose);
		if(choose == 'Y' || choose == 'y'){
			fd_dst = open(argv[2],O_RDWR);
			if(fd_dst == -1)perror("open"),exit(1);
		}else{
			exit(0);
		}
	}
	
	//循环读取源文件，写入目标文件
	char buf[1024+1];
	while(1){
		memset(buf, 0x00 , sizeof(buf));
		int r = read(fd_src, buf, 1024);
		
		if(r <= 0)break;
		
		write(fd_dst, buf, r);
	}

	//关闭文件
		
	close(fd_src);
	close(fd_dst);
}
```