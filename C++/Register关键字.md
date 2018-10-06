### Register关键字

​	请求编译器将变量直接放在寄存器中，这样速度快。

#### C语言中的Register

​	在C语言中，register修饰变量  不能取地址，但是 在C++中有所不同；

```c
	#include<stdio.h>
	
	int main()
	{
		register int i = 1;
		printf("&a=%d",&i);		//编译器报错  提示你不能对register变量取地址
	}
```



#### C++中的register

​	1、在C++中仍然支持register关键字，但是C++编译器有自己的优化方式，即使你自己不使用register

编译器在编译的时候会对你的程序做优化，优化后可能使用register关键字。

​	2、C++编译器发现对register变量取地址时，register会对变量的修饰变的无效，仍然可以取出地址，但是现在改变量已经不是register变量了。

​	3、早期的C编译器不会对代码进行优化，所以register关键字是一个很好的补充。

```C++
	#include<iostream>
	
	int main()
	{
		register int i = 1;
		cout<<"&a="<<&i<<endl;		//编译器不会报错  并打印出 i 的地址
	}
```

