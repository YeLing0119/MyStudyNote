### **namespace**

​	所谓namespace，是指标识符的可见范围。C++标准库中的所有标识符都被定义在一个名为 std 的namespace 中。

**一、\<iostream>和<iostream.h>格式不一样，前者没有后缀。**实际上，在你自己编译器的include路径下，你可以看到这两个是两个不同的文件，而且打开后里面的代码也不同。后缀为 .h 的头文件C++标准以明确提出不支持了，C++标准为了与C区分开，也为了能够正确的使用命名空间，规定了头文件不适用后缀  .h 。因而

​	1、当使用\<iostream.h>时，相当于在 c 中调用库函数，使用的是全局命名空间，也就是早期的C++实现；

​	2、当使用\<iostream>时，该头文件没有定义全局命名空间，必须使用 namespace std ; 这样才能够正确的使用 cout ；

**二、 由于namespace的概念，使用C++标准库中的标识符时，有以下三种方式：**

​	1、直接在标识符前加上命名空间std  :  例如  std :: << cout << std :: hex << 3.1415 << std :: endl ;

​	2、用 using 关键字指定部分标识符的命名空间：using std :: cout ; using std :: endl ; 那么1中的语句可以改写为 cout << std :: hex << 3.1415 << endl ;

​	3、最方便的就是使用 using namespace std ; 让命名空间 std 内的所有标识符都有效 。好像把他声明成全局函数一样，这样上面的代码就可以写成 cout << hex << 3.1415 << endl ;

**三、命名空间的定义**

​	namespace name { ... } ;

**四、命名空间的使用**

```c++
#include <iostream>
using namespace std;

namespace namespaceA
{
    int a = 0 ;
    namespace namespaceD
    {
    	int b;
    }    	
}

namespace namespaceB
{
    int a = 0 ;
    namespace namespaceC
    {
    	int b;
    }
}

int main()
{
    using namespace namespaceA;
    using namespace namespaceB;
	cout << a << endl;  
}
```

这样编译，会报错，提示你 a 的命名空间不明确，改成以下即可正确运行：

```C++
cout << namespaceA :: a << end ;
cout << namespaceB :: a << end ;
```



注意，namespace的定义可以嵌套，比如要访问 namespaceC 中的 b 可以这样写

```C++
using namespace namespaceB::namespaceC;
cout << b << endl ;  //这样就可以访问 namespaceC 中的 b 了；
```

